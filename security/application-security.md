# Application Security

## General principles

- [Defense in Depth](<https://en.wikipedia.org/wiki/Defense_in_depth_(computing)>)
- [Principle of least priviledge](https://en.wikipedia.org/wiki/Principle_of_least_privilege)

## Encryption in transit

All API services produced at OGP should have encryption in transit.

Web APIs and sites should only be accessed over SSLs. Mechanisms should be implemented to redirect callers towards the secure channels (http to https)

## Database access and schema migration

Application should access databases with credentials granting the minimum access needed for run time operations. Required permissions typically include insert/update/delete on tables ROWS.

Under most circumstances, applications do not need to manipulate the database schema. Access should therefore not grant schema manipulation rights.

Since schema migration are typically a rare occurence compared to "normal" run-time operations. Schema migration should be managed as ad-hoc events with a custom process,
