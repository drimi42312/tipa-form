# Tipa Form — Security Plan (Medical Data)

## Context
This form will collect **medical information** and requires elevated security measures beyond a standard web form.

## Legal Requirements (Israel)
- Compliance with **חוק הגנת הפרטיות** (Privacy Protection Law)
- Explicit user **consent** before collecting medical data
- Defined **data retention policy** (how long stored, who can access)
- Privacy policy page required

## Planned Architecture

```
Frontend (HTML form)
  → HTTPS only
  → POST /api/submit  (Express, JWT-protected)
    → Field-level encrypted storage (PostgreSQL)
    → Audit log entry
    → Telegram/email notification to practitioner
```

## Security Measures

### Authentication
- [ ] Individual logins per user (no shared passwords)
- [ ] Two-factor authentication (2FA) mandatory
- [ ] Session timeout after inactivity
- [ ] Audit log: who accessed what + timestamp

### Data
- [ ] Encrypt sensitive fields at rest (field-level encryption)
- [ ] HTTPS only — no plain HTTP
- [ ] No medical data stored in Notion (not HIPAA-compliant)
- [ ] No medical data in server logs
- [ ] Encrypted backups

### Infrastructure
- [ ] Dedicated VPS (not shared hosting)
- [ ] Rate limiting on form endpoint
- [ ] Environment variables for all secrets

## What NOT to Use for Medical Data
- ❌ Notion (not certified for medical data)
- ❌ WhatsApp for transmitting patient info
- ❌ Public GitHub repo for any patient data or keys

## Open Questions
- Who fills the form? (patients / caregivers / staff)
- What type of data? (intake / assessment / referral)
- Who accesses results? (single doctor / clinic team)
- Expected scale? (tens / hundreds / thousands of submissions)

## Stack Recommendation
- **Backend:** Node.js + Express (existing infrastructure)
- **Database:** PostgreSQL with field-level encryption
- **Auth:** JWT + 2FA (TOTP via Google Authenticator)
- **Hosting:** Dedicated VPS or Israeli certified cloud
