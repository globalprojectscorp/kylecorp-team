# Design Principles & Defaults

## Default Design System
**When in doubt, follow the Apple Human Interface Guidelines (HIG).**

Apple has done careful, considered work on UI/UX design. The HIG is the default unless explicitly specified otherwise.

Reference: https://developer.apple.com/design/human-interface-guidelines/

## Typography
**Default font family:** Neue Haas Grotesk

- **Preferred variant:** Rounded
- **Display vs Text:** Use Display for headlines and large text, Text for body and readable content
- Font files located in `~/.claude/Fonts/`

## Apple Platform Development (Xcode, iOS, macOS, etc.)
- Strictly follow Apple HIG
- Use standard SwiftUI components and design patterns unless otherwise specified
- Prefer native implementations over third-party libraries
- Follow platform conventions for navigation, layout, and interaction

## Figma Integration
When working with Figma files:
- Extract design tokens (colors, spacing, typography) and sync to code
- Map Figma components to code components consistently
- Maintain naming parity between Figma and codebase
- Use Figma as source of truth for visual specs; code implements the vision
- When translating designs, apply Apple HIG refinement — Figma designs pass through the same quality filter
