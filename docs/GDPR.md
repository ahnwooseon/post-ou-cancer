# GDPR Compliance Requirements

This document outlines the GDPR (General Data Protection Regulation) compliance requirements for the Post Ou Cancer platform.

## Overview

The Post Ou Cancer platform must comply with GDPR requirements for processing personal data of EU residents. This includes data collection, storage, processing, export, deletion, and user consent management.

## Key GDPR Principles

1. **Lawfulness, fairness, and transparency**: Users must be informed about data processing
2. **Purpose limitation**: Data collected only for specified purposes
3. **Data minimization**: Collect only necessary data
4. **Accuracy**: Keep data accurate and up to date
5. **Storage limitation**: Retain data only as long as necessary
6. **Integrity and confidentiality**: Secure data processing
7. **Accountability**: Demonstrate compliance
8. **Privacy by Design and by Default**: Embed data protection into technology

## User Rights

### 1. Right to Access (Article 15)

**Status**: ⚠️ To be implemented

Users have the right to access their personal data.

**Implementation**:

- [ ] API endpoint: `GET /api/me/data-export` (user identified via auth token)
- [ ] Export all user data in a structured, commonly used, and machine-readable format (JSON)
- [ ] Include:
  - Profile information
  - Posts and comments
  - Votes and reactions
  - Messages
  - Activity logs
  - Privacy settings
- [ ] Response time: Within 30 days (GDPR requirement)

**Data Export Format**:

```json
{
  "userId": "uuid",
  "exportDate": "2025-11-16T12:00:00Z",
  "profile": { ... },
  "posts": [ ... ],
  "comments": [ ... ],
  "votes": [ ... ],
  "messages": [ ... ],
  "activityLog": [ ... ],
  "settings": { ... }
}
```

---

### 2. Right to Rectification (Article 16)

**Status**: ⚠️ To be implemented

Users have the right to correct inaccurate personal data.

**Implementation**:

- [ ] Allow users to update their profile information
- [ ] API endpoint: `PUT /api/me/profile` (user identified via auth token)
- [ ] Validation of updated data
- [ ] Audit trail of changes

---

### 3. Right to Erasure / "Right to be Forgotten" (Article 17)

**Status**: ⚠️ To be implemented

Users have the right to request deletion of their personal data.

**Implementation**:

- [ ] API endpoint: `DELETE /api/me/data` (user identified via auth token)
- [ ] Soft delete: Mark user account as deleted
- [ ] Anonymize or delete:
  - User profile
  - Posts (option: anonymize or delete)
  - Comments (option: anonymize or delete)
  - Messages (anonymize sender in recipient's inbox)
  - Activity logs
- [ ] Hard delete after retention period (30 days grace period)
- [ ] Cascade deletion of related data
- [ ] Audit trail of deletion requests

**Deletion Strategy**:

1. Immediate soft delete (account marked as deleted)
2. 30-day grace period (user can restore account)
3. Anonymization of public content (optional, user choice)
4. Hard delete after grace period
5. Backup retention: 90 days (for disaster recovery)

**Exceptions** (data that may be retained):

- Legal obligations (tax records, court orders)
- Legitimate interests (fraud prevention)
- Public interest (archived public posts)

---

### 4. Right to Restrict Processing (Article 18)

**Status**: ⚠️ To be implemented

Users can request restriction of data processing.

**Implementation**:

- [ ] API endpoint: `POST /api/me/restrict-processing` (user identified via auth token)
- [ ] Suspend account activity
- [ ] Stop data processing while maintaining data
- [ ] Notification when restriction is lifted

---

### 5. Right to Data Portability (Article 20)

**Status**: ⚠️ To be implemented

Users have the right to receive their data in a structured, machine-readable format.

**Implementation**:

- [ ] Same implementation as Right to Access (data export).
- [ ] JSON format (machine-readable)
- [ ] The export should include all data provided by the user, both actively (e.g., profile, posts) and observed (e.g., activity logs), to facilitate transfer to another service.
- [ ] Include all user-generated content

---

### 6. Right to Object (Article 21)

**Status**: ⚠️ To be implemented

Users can object to processing of their personal data.

**Implementation**:

- [ ] Opt-out mechanisms for:
  - Marketing communications
  - Analytics tracking
  - Profiling
- [ ] API endpoint: `POST /api/me/object-processing` (user identified via auth token)
- [ ] Respect user preferences immediately

---

### 7. Rights Related to Automated Decision-Making (Article 22)

**Status**: ⚠️ To be implemented

Users have rights regarding automated decision-making and profiling.

**Implementation**:

- [ ] Transparency about automated decisions (AI moderation)
- [ ] Right to human review of automated decisions
- [ ] Opt-out from automated profiling
- [ ] Explainability of AI decisions

---

## Consent Management

**Status**: ⚠️ To be implemented

### Consent Requirements

- [ ] **Explicit consent**: Clear, affirmative action required
- [ ] **Granular consent**: Separate consent for different purposes
- [ ] **Withdrawable consent**: Easy to withdraw at any time
- [ ] **Consent records**: Track consent history
- [ ] **Age verification**: Verify user age (16+ in EU, or parental consent). Specify method (e.g., self-declaration) and process for parental consent.

### Consent Types

1. **Account Creation**: Consent to Terms of Service and Privacy Policy
2. **Data Processing**: Consent to process personal data
3. **Marketing**: Consent to receive marketing communications
4. **Analytics**: Consent to analytics tracking
5. **Cookies**: Cookie consent (if applicable)
6. **Third-party sharing**: Consent to share data with third parties

### Implementation

- [ ] Consent management UI in user settings
- [ ] API endpoints for consent management
- [ ] Consent audit trail
- [ ] Consent withdrawal mechanism
- [ ] Default: Opt-in (not opt-out)

**Consent Data Model**:

```csharp
public class Consent
{
    public Guid UserId { get; set; }
    public ConsentType Type { get; set; }
    public bool Granted { get; set; }
    public DateTime GrantedAt { get; set; }
    public DateTime? WithdrawnAt { get; set; }
    public string PolicyVersion { get; set; } // Version of the policy text (e.g., "v1.1-2025-11-16")
    public string IpAddress { get; set; }
}
```

---

## Data Processing Activities

### Lawful Basis for Processing

1. **Consent** (Article 6(1)(a)): User has given consent
2. **Contract** (Article 6(1)(b)): Necessary for service provision
3. **Legal obligation** (Article 6(1)(c)): Required by law
4. **Legitimate interests** (Article 6(1)(f)): Platform operation (e.g., fraud prevention, network security, service improvement analytics)

### Data Categories

1. **Identity Data**: Name, email, username
2. **Profile Data**: Bio, avatar, preferences
3. **Content Data**: Posts, comments, messages
4. **Activity Data**: Votes, views, interactions
5. **Technical Data**: IP address, device info, logs
6. **Marketing Data**: Preferences, consent records

---

## Data Retention

**Status**: ⚠️ To be implemented

### Retention Periods

- **Active user data**: Retained while account is active
- **Deleted account data**: 30-day grace period, then hard delete
- **Backup data**: 90 days (for disaster recovery)
- **Logs**: 6 months (security and compliance)
- **Analytics data**: 2 years (anonymized)
- **Legal obligations**: As required by law

### Data Minimization

- [ ] Collect only necessary data
- [ ] Regular data cleanup of unused data
- [ ] Anonymize data when no longer needed in identifiable form

---

## Data Breach Notification

**Status**: ⚠️ To be implemented

### Requirements

- [ ] **Detection**: Monitor for data breaches
- [ ] **Assessment**: Evaluate risk to individuals
- [ ] **Notification to authorities**: Within 72 hours (Article 33)
- [ ] **Notification to users**: Without undue delay if high risk (Article 34)
- [ ] **Documentation**: Record all breaches

### Breach Response Plan

1. Contain the breach
2. Assess the risk
3. Notify authorities (DPA) within 72 hours
4. Notify affected users if high risk
5. Document the incident
6. Review and improve security measures

---

## Privacy by Design

**Status**: ⚠️ To be implemented

- [ ] Privacy considerations in system design
- [ ] Data minimization by default
- [ ] Encryption by default
- [ ] Access controls by default
- [ ] Privacy impact assessments for new features

---

## Data Protection Officer (DPO)

**Status**: ⚠️ To be determined

- Determine if DPO is required (based on processing scale)
- If required, appoint DPO and publish contact information

---

## Third-Party Data Sharing

**Status**: ⚠️ To be implemented

- [ ] Document all third-party data processors
- [ ] Data Processing Agreements (DPA) with processors
- [ ] User consent for third-party sharing
- [ ] Regular audits of third-party processors

---

## Audit and Compliance

**Status**: ⚠️ To be implemented

- [ ] Regular GDPR compliance audits
- [ ] Documentation of data processing activities
- [ ] Privacy impact assessments
- [ ] Training for staff on GDPR requirements

---

## Implementation Checklist

- [ ] User data export functionality
- [ ] User data deletion functionality
- [ ] Consent management system
- [ ] Privacy policy and terms of service
- [ ] Cookie consent (if applicable)
- [ ] Data breach notification procedures
- [ ] Data retention policies
- [ ] Audit logging of data access
- [ ] User rights request handling
- [ ] Age verification mechanism

---

## References

- [GDPR Official Text](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32016R0679)
- [ICO GDPR Guide](https://ico.org.uk/for-organisations/guide-to-data-protection/guide-to-the-general-data-protection-regulation-gdpr/)
- [EDPB Guidelines](https://edpb.europa.eu/our-work-tools/general-guidance/gdpr-guidelines-recommendations-best-practices_en)
