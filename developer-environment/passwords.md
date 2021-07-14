# Passwords and credentials

Developers are expected to use their 1Password account to generate and save *all* passwords and secrets from their time the organisation.

## Personal credentials

Personal account credentials scoped to you should be *generated* and *stored* in the Private vault. Examples include their AWS account, Cloudflare account etc.

Where 2 Factor Authentication is used, developers are *encouraged* to use Authenticator on their mobile devices instead of 1Password.

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

Production credentials are secrets used by production systems. These should be stored in the relevant production vault for the project. Examples include Twilio API keys or user-specific API keys.

Only full-time employees of the relevant project team should have access to these credentials - interns and volunteers should not.

### Staging credentials

Staging credentials are secrets used by staging environments. These should be stored in the relevant staging vault for the project, **separately from production credentials**.

Apart from full-time employees, interns can also have access to these credentials. This allows interns to have the same development experience as full-timers, without being able to cause lasting harm to the organisation.
