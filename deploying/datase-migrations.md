# Database Schema migration

Schema migrations are not part of the "normal" operations of applications, and application database acces should therefore not grant schema manipulation permissions (for more details see the [Application Security](../security/application-security.md))

Since applications will not be able to update the schema themselves, migrations should be handled as a separate process, to be considered and undertaken as part of releases which introduce and need schema changes.

To minimize human errors, changes should be scripted and executed in pairs.

## Roll-Forward / Roll-Back

Any depoyment must assume a risk of incident and rollback. Database migrations constitute an additional layer of risk to consider, with the biggest issue arising when 2 versions of an application are not compatible with a shared schema.

Some migrations are not problematic from the perspective of a rollback. For example the addition of a new column in a table (typically) doesn't affect an older version of the code, which would just ignore the column. Some are blockers however: removing columns or tables, adding constraints may cause appplications to throw run-time errors.

Where possible, migrations should be be considered as part of a multi-deployments process:

1. Deploy a parity version of the application that is compatible with the current and new schema
2. Perform the schema update
3. Implement the desired code changes
4. When stability is verified, clean up backward compatibilty code and schema

Migrations can be complex based on the changes being applied. The guideline above may not apply to all situations. Dev teams should discuss carefully, and document their plan of action for complex migrations cases.
