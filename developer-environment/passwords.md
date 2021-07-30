# Passwords and credentials

Developers are expected to use their 1Password account to generate and save *all* passwords and secrets from their time the organisation.

## Personal credentials

Personal account credentials are credentials that are scoped to you as an individual.

As far as possible, this should be *generated* and *stored* in the Private vault. Examples include your AWS account or Cloudflare account etc.

## Project credentials

Project credentials are broadly categorised into three types:

- Root credentials
- Production credentials
- Staging and UAT credentials

### Root credentials

Root credentials are superadmin credentials that allow broad access to production systems. Some examples include AWS root credentials or Cloudflare root credentials.

Because any leak of root credentials are extremely damaging, broad sharing of such credentials is **strongly discouraged**. Such credentials - once exploited - are **highly damaging** and may cause irrecoverable harm not only to production systems, users and citizens. As such, only the technical lead and product manager of your team should have access to such credentials.

If you observe the frequent use of root credentials, please sound it out to your teams.

### Production credentials

Production credentials are secrets used by production systems. These should be stored in the relevant production vault for the project. Examples include Twilio API keys used in production to send SMSes.

API keys used in production **should not** be shared with staging or other non-production environments.

Only full-time employees of the relevant project team should have access to these credentials - interns and volunteers should not.

### Staging credentials

Staging credentials are secrets used by staging environments. These should be **stored in the relevant staging vault for the project, separately from production credentials**.

Apart from full-time employees, interns on the relevant project team can also have access to these credentials. This allows interns to have a similar development experience as full-time employees, without having access to credentials that can cause lasting harm to the organisation.

## Two-factor authentication (2FA)

Developers are **strongly encouraged** to enable 2FA when available. The 2nd factor should be stored separately from 1Password, using e.g. Authenticator on your mobile device.
