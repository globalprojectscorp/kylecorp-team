---
name: testflight
description: Build and deploy iOS apps to TestFlight. Use when the user says "put this on testflight", "testflight build", "push to testflight", "deploy to testflight", "ship it to testflight", or any variation of getting an iOS app onto TestFlight. Handles the entire pipeline — app creation, building, signing, and uploading — with zero configuration needed from the user.
---

# TestFlight Deployment

Zero-config pipeline. Do NOT ask the user anything — detect everything automatically and run it.

## Credentials (baked in, never ask)

- **API Key ID:** `$ASC_API_KEY_ID`
- **API Key Issuer ID:** `$ASC_API_KEY_ISSUER_ID`
- **API Key File:** `~/.appstoreconnect/private_keys/AuthKey_$ASC_API_KEY_ID.p8`
- **Team ID:** `$APPLE_TEAM_ID`
- **Apple ID:** Set in `~/.zshrc` as `EXPO_APPLE_ID` (loaded from System keychain)
- **Apple ID Password:** Set in `~/.zshrc` as `EXPO_APPLE_PASSWORD` (loaded from System keychain, used by EAS for app creation in Connect)

## Pre-check

Run silently before starting. If any fail, stop and tell the user what's wrong — do NOT try to fix infrastructure problems.

```bash
test -f ~/.appstoreconnect/private_keys/AuthKey_$ASC_API_KEY_ID.p8 || echo "FAIL: API key missing"
xcodebuild -version 2>&1 | head -1 || echo "FAIL: Xcode not installed"
eas whoami 2>&1 || echo "FAIL: EAS not logged in — run: eas login"
security find-identity -v -p codesigning /Library/Keychains/System.keychain 2>&1 | grep -q "valid" || echo "FAIL: No signing certs in System keychain"
```

## Step 1: Detect the project

Find the Xcode project or workspace:

```bash
# Prefer workspace (CocoaPods, SPM with custom packages), fall back to xcodeproj
WORKSPACE=$(find . -maxdepth 2 -name "*.xcworkspace" -not -path "*/DerivedData/*" -not -path "*/.build/*" -not -path "*/xcodeproj/*" | head -1)
XCODEPROJ=$(find . -maxdepth 2 -name "*.xcodeproj" -not -path "*/DerivedData/*" | head -1)

# Use workspace if it exists, otherwise project
if [ -n "$WORKSPACE" ]; then
  BUILD_TARGET="-workspace $WORKSPACE"
else
  BUILD_TARGET="-project $XCODEPROJ"
fi
```

List schemes and pick one:

```bash
xcodebuild -list $BUILD_TARGET 2>/dev/null
```

**Choosing the scheme:** Pick the scheme that represents the shipping app. Ignore schemes named for tests (`*Tests`), frameworks (`SwiftShell`, `Pods-*`), or helper targets. If there's one scheme matching the app name, use that. If there are Debug/Staging/Production variants, prefer Production or the non-suffixed one. If uncertain, use the scheme matching the project name.

Get build settings from the chosen scheme:

```bash
xcodebuild -showBuildSettings $BUILD_TARGET -scheme "<chosen scheme>" 2>/dev/null | grep -E "PRODUCT_BUNDLE_IDENTIFIER|PRODUCT_NAME|MARKETING_VERSION|CURRENT_PROJECT_VERSION"
```

## Step 2: Register in App Store Connect (skip if app.json exists)

The App Store Connect API does not support creating new apps — EAS handles this.

Create `app.json` (substitute real values from step 1):
```json
{
  "expo": {
    "name": "<PRODUCT_NAME>",
    "slug": "<last segment of PRODUCT_BUNDLE_IDENTIFIER>",
    "ios": { "bundleIdentifier": "<PRODUCT_BUNDLE_IDENTIFIER>" },
    "extra": { "eas": { "projectId": "" } }
  }
}
```

Create `eas.json`:
```json
{
  "cli": { "appVersionSource": "local" },
  "submit": {
    "production": {
      "ios": {
        "appleId": "$APPLE_ID",
        "ascApiKeyPath": "~/.appstoreconnect/private_keys/AuthKey_$ASC_API_KEY_ID.p8",
        "ascApiKeyId": "$ASC_API_KEY_ID",
        "ascApiKeyIssuerId": "$ASC_API_KEY_ISSUER_ID",
        "appleTeamId": "$APPLE_TEAM_ID"
      }
    }
  }
}
```

```bash
eas init --non-interactive --force
```

## Step 3: Bump build number

Query TestFlight for the latest uploaded build number (not the project — TestFlight might be ahead):

```bash
# Get latest build number from App Store Connect
LATEST=$(xcrun altool --list-apps --apiKey $ASC_API_KEY_ID --apiIssuer $ASC_API_KEY_ISSUER_ID 2>/dev/null | grep -A5 "<PRODUCT_BUNDLE_IDENTIFIER>" | grep -o 'previousBundleVersion.*' | head -1)

# If that doesn't work, read from project and add 1
CURRENT=$(xcodebuild -showBuildSettings $BUILD_TARGET -scheme "<scheme>" 2>/dev/null | grep CURRENT_PROJECT_VERSION | tr -d ' ' | cut -d= -f2)
NEXT=$((CURRENT + 1))

# Set the new build number
xcrun agvtool new-version -all $NEXT
```

If `agvtool` doesn't work (project doesn't use Apple Generic Versioning), find the Info.plist and use PlistBuddy:
```bash
/usr/libexec/PlistBuddy -c "Set :CFBundleVersion $NEXT" <path to Info.plist>
```

## Step 4: Archive

```bash
rm -rf ./build/App.xcarchive
xcodebuild archive \
  $BUILD_TARGET \
  -scheme "<scheme>" \
  -configuration Release \
  -destination "generic/platform=iOS" \
  -archivePath ./build/App.xcarchive \
  -allowProvisioningUpdates \
  -authenticationKeyPath ~/.appstoreconnect/private_keys/AuthKey_$ASC_API_KEY_ID.p8 \
  -authenticationKeyID $ASC_API_KEY_ID \
  -authenticationKeyIssuerID $ASC_API_KEY_ISSUER_ID \
  DEVELOPMENT_TEAM=$APPLE_TEAM_ID \
  CODE_SIGN_STYLE=Automatic
```

## Step 5: Export IPA

Create `ExportOptions.plist` if it doesn't already exist in the project:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>method</key>
    <string>app-store-connect</string>
    <key>teamID</key>
    <string>$APPLE_TEAM_ID</string>
    <key>signingStyle</key>
    <string>automatic</string>
    <key>manageAppVersionAndBuildNumber</key>
    <false/>
    <key>uploadSymbols</key>
    <true/>
</dict>
</plist>
```

```bash
rm -rf ./build/export
xcodebuild -exportArchive \
  -archivePath ./build/App.xcarchive \
  -exportPath ./build/export \
  -exportOptionsPlist ExportOptions.plist \
  -allowProvisioningUpdates \
  -authenticationKeyPath ~/.appstoreconnect/private_keys/AuthKey_$ASC_API_KEY_ID.p8 \
  -authenticationKeyID $ASC_API_KEY_ID \
  -authenticationKeyIssuerID $ASC_API_KEY_ISSUER_ID
```

## Step 6: Upload to TestFlight

Use `eas submit` — NOT `xcrun altool`. EAS handles both creating the app in App Store Connect (if it doesn't exist yet) AND uploading the IPA. `altool` cannot create apps and will fail on first upload.

Apple ID credentials are pre-configured as environment variables in `~/.zshrc` (`EXPO_APPLE_ID` and `EXPO_APPLE_PASSWORD`), and the `appleId` field is in eas.json. This means EAS can authenticate with Apple without prompting.

**IMPORTANT:** `--non-interactive` skips app creation entirely — it only works for apps that already exist in Connect. For new apps, you MUST omit `--non-interactive` so EAS runs the "ensure app exists" step. The env vars prevent it from prompting for credentials.

```bash
IPA=$(find ./build/export -name "*.ipa" | head -1)

# Source env vars, then submit WITHOUT --non-interactive
# (EAS uses env vars for auth — no prompts even without the flag)
source ~/.zshrc 2>/dev/null
eas submit --platform ios --path "$IPA"
```

After the first successful upload, EAS saves `ascAppId` in eas.json. Future uploads can then use `--non-interactive` if desired, but omitting it is fine — the env vars prevent prompts either way.

## Step 7: Done

Tell the user the build is uploaded and processing. It appears in TestFlight in 5-15 minutes. Report the version and build number.

Add `build/` to `.gitignore` if not already there. Commit `app.json`, `eas.json`, and `ExportOptions.plist` if they were created.

## If Something Fails

**Read the error. Do not start changing project settings, keychain configs, or signing configurations.** The pipeline either works as-is or there's a specific, diagnosable problem. Here are the only acceptable responses to failures:

| Error | Fix |
|-------|-----|
| `Scheme not found` | Re-run `xcodebuild -list` — you picked the wrong scheme name (case-sensitive) |
| `Build number already exists` | Bump higher in step 3 and retry from step 4 |
| `No signing certificate` | Signing certs are missing from System keychain. Tell user — do NOT try to fix it |
| `No profiles for bundle ID` | The `-allowProvisioningUpdates` flag should auto-create profiles. If it fails, the bundle ID doesn't match what's in App Store Connect. Check step 2. |
| `App record not found` / altool says app doesn't exist | Do NOT use `xcrun altool`. Use `eas submit` as shown in step 6 — it creates the app in Connect automatically. |
| `SPM packages fail to resolve` | Add `-clonedSourcePackagesDirPath ./build/spm` to the archive command |
| `EAS not logged in` | Tell user to run `eas login` |
| Archive compiles but signing fails | Tell user — do NOT manipulate keychains, create certs, or revoke anything |
| Any other error | Report it to the user. Do NOT improvise fixes. |

## Rules

- **NEVER ask the user for credentials, confirmation, or configuration.** Everything is pre-configured.
- **NEVER manipulate keychains, create keychains, delete certs, revoke certs, or import p12 files.** If signing is broken, tell the user — period.
- **NEVER modify the Xcode project's signing settings** (CODE_SIGN_IDENTITY, PROVISIONING_PROFILE_SPECIFIER, etc. in the pbxproj). Only pass build settings as command-line overrides.
- **NEVER use Fastlane, Puppeteer, or osascript wrappers.**
- **Use `--non-interactive --force` with `eas init`** but **NOT with `eas submit`** (non-interactive skips app creation for new apps).
- **ALWAYS use `-allowProvisioningUpdates` and the three API key auth flags** on every xcodebuild command.
- **ALWAYS clean `./build/` before archiving** to avoid stale artifacts.
- **If something unexpected happens, STOP and tell the user.** Do not start experimenting with alternative approaches, different signing methods, or workarounds. The pipeline is proven — if it fails, something specific is wrong.
