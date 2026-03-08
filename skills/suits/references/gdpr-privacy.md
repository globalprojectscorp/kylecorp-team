# GDPR & Privacy Compliance — Suits Reference

## When to Load
Load this reference when:
- The project handles personal data (names, emails, IPs, behavior data)
- User asks about GDPR, CCPA, HIPAA, privacy policies, or data compliance
- Evaluating consent mechanisms, data retention, or breach readiness
- Reviewing contracts with data processing implications

## GDPR Compliance Framework

### Data Categories & Retention

```yaml
gdpr_compliance:
  data_categories:
    personal_identifiers:
      types: [name, email, phone_number, ip_address]
      retention_period: "2 years"
      legal_basis: "contract"
    behavioral_data:
      types: [website_interactions, purchase_history, preferences]
      retention_period: "3 years"
      legal_basis: "legitimate_interests"
    sensitive_data:
      types: [health_information, financial_data, biometric_data]
      retention_period: "1 year"
      legal_basis: "explicit_consent"
      special_protection: true

  data_subject_rights:
    right_of_access:
      response_time: "30 days"
      procedure: "automated_data_export"
    right_to_rectification:
      response_time: "30 days"
      procedure: "user_profile_update"
    right_to_erasure:
      response_time: "30 days"
      procedure: "account_deletion_workflow"
      exceptions: [legal_compliance, contractual_obligations]
    right_to_portability:
      response_time: "30 days"
      format: "JSON"
      procedure: "data_export_api"
    right_to_object:
      response_time: "immediate"
      procedure: "opt_out_mechanism"

  breach_response:
    detection_time: "72 hours"
    authority_notification: "72 hours"
    data_subject_notification: "without undue delay"

  privacy_by_design:
    data_minimization: true
    purpose_limitation: true
    storage_limitation: true
    accuracy: true
    integrity_confidentiality: true
    accountability: true
```

### Privacy Policy Required Sections

1. **Data Collection** — What we collect:
   - Provided directly: Account Info, Profile Data, Transaction Data, Communication Data
   - Collected automatically: Usage Data, Device Info, Location Data, Cookie Data

2. **User Rights by Jurisdiction:**
   - **GDPR**: Access, Rectification, Erasure, Restrict Processing, Data Portability, Object, Withdraw Consent (30-day response)
   - **CCPA**: Right to Know, Delete, Opt-Out, Non-Discrimination (45-day response)

3. **Compliance Validation Checklist:**
   - [ ] Legal basis documented for each data category
   - [ ] Retention periods defined and enforced
   - [ ] User rights mechanisms implemented and tested
   - [ ] DPO contact information published
   - [ ] Breach notification procedure documented
   - [ ] Clear language (no legalese)
   - [ ] Contact information provided
   - [ ] Effective date and update mechanism
   - [ ] Third-party sharing disclosed
   - [ ] Cookie consent mechanism functional

### Contract Risk Assessment

**High-risk keywords** (score: 3 each):
- unlimited liability, personal guarantee, indemnification, liquidated damages, injunctive relief, non-compete

**Medium-risk keywords** (score: 2 each):
- intellectual property, confidentiality, data processing, termination rights, governing law, dispute resolution

**Risk scoring:** Total >= 10 = HIGH (legal review required), >= 5 = MEDIUM (manager approval), < 5 = LOW

**Standard recommendations:**
- Mutual liability caps at 12 months of fees (HIGH priority)
- Termination for convenience with 30-day notice (MEDIUM)
- Data return and deletion provisions (HIGH)

### Regulatory Quick Reference

| Regulation | Scope | Key Requirement | Penalty |
|-----------|-------|-----------------|---------|
| GDPR | EU residents' data | Consent, DPO, breach notification | Up to 4% global revenue |
| CCPA/CPRA | California residents | Right to know, delete, opt-out | $7,500 per violation |
| HIPAA | Health data (US) | Encryption, access controls, BAAs | Up to $1.5M per violation |
| PCI-DSS | Payment card data | Encryption, network security, logging | Fines + loss of processing |
| SOX | Public companies (US) | Financial controls, audit trails | Criminal penalties |

## Review Output Format

```markdown
### Compliance Assessment
- **Regulatory Exposure:** [GDPR / CCPA / HIPAA / None identified]
- **Risk Level:** [Low / Medium / High / Critical]
- **Data Categories Handled:** [List]
- **Findings:**
  1. **[Finding]** — [Regulatory risk]
     - Regulation: [e.g., GDPR Art. 17]
     - Current state: [What's happening now]
     - Required state: [What compliance requires]
     - Fix: [Specific action + timeline]
```
