Create a clean, professional enterprise architecture infographic explaining Oracle Database common-schema access for a development/testing team.

Title:
“Oracle Common Schema Access – What Is Possible, What Is Restricted, and Available Options”

Use a simple Oracle Database icon at the top and divide the diagram into THREE clearly separated sections.

SECTION 1 — POSSIBLE / RECOMMENDED
Heading: “Common Schema + Individual User Access”

Show one central schema:
TEAM_SCHEMA
(Owns Tables, Views, Procedures)

Show 5 individual database users:
User 1 – Lavanya
User 2 – Thomas
User 3
User 4
User 5

Each person logs in using their OWN username.

Between the users and TEAM_SCHEMA, show a shared database ROLE:
TEAM_SCHEMA_ROLE

Show that the role can provide object-level privileges such as:
✓ SELECT
✓ INSERT
✓ UPDATE
✓ DELETE
✓ EXECUTE

Clearly label:
“Individual login and accountability maintained”

Show:
Users → Role → Existing Objects in TEAM_SCHEMA

Use green check marks to indicate this is supported and recommended.

SECTION 2 — ORACLE LIMITATION
Heading: “What We Cannot Restrict Cleanly”

Show an individual user trying to create a new object:

Lavanya
↓
CREATE TABLE TEAM_SCHEMA.TEST_TABLE

Put a red restriction symbol between the user and TEAM_SCHEMA.

Explain visually:

Normal CREATE TABLE
→ Creates objects in the user’s OWN schema.

Creating objects in another schema may require broader privileges such as:
CREATE ANY TABLE
CREATE ANY VIEW
CREATE ANY PROCEDURE

Show those privileges branching toward MULTIPLE schemas:
Schema R
Schema M
Schema D
TEAM_SCHEMA
Other Schemas

Add a red warning:
“CREATE ANY is broader than TEAM_SCHEMA. It is not a simple privilege restricted only to this common schema.”

Main limitation statement:
“We cannot simply grant a user unrestricted object-creation capability and scope it only to one other schema using the normal Oracle privilege model.”

SECTION 3 — ALTERNATIVES
Heading: “Available Alternatives”

Show three alternatives side-by-side.

Alternative A — Shared Schema Account
Create:
TEAM_SCHEMA
Username + Password

Show all 5 team members connecting using the same TEAM_SCHEMA credentials.

Benefits:
✓ Can create/manage objects owned by TEAM_SCHEMA
✓ No CREATE ANY privilege required

Trade-off:
⚠ Shared database identity
⚠ Reduced individual accountability/audit clarity
⚠ Shared credential/security considerations

Label:
“Technically Simple – Requires Security Approval”

Alternative B — Individual Users + Controlled Object Creation
Show:
Lavanya
Thomas
User 3
User 4
User 5
↓
TEAM_SCHEMA_ROLE
↓
TEAM_SCHEMA

Users receive:
SELECT / INSERT / UPDATE / DELETE / EXECUTE

For CREATE/DROP/ALTER requests show:
User → DBA / Schema Owner → TEAM_SCHEMA

Benefits:
✓ Individual accountability
✓ Least privilege
✓ Better security

Label this:
“RECOMMENDED / SAFEST”

Alternative C — Individual Schemas
Show:
Lavanya → LAVANYA_SCHEMA
Thomas → THOMAS_SCHEMA
User 3 → USER3_SCHEMA

Each user can create their own tables/views/procedures in their own schema.

Benefits:
✓ Independent development
✓ Individual accountability
✓ No shared password

Trade-off:
⚠ Not one common working schema

At the bottom add a decision flow:

“Do users need to CREATE/DROP/ALTER objects?”

NO
→ Common Schema + Individual Users + Role
→ RECOMMENDED

YES
→ “Must everyone create objects directly in the SAME schema?”

NO
→ Individual Schemas

YES
→ Choose between:
1. Shared TEAM_SCHEMA account — simpler but weaker individual accountability
OR
2. DBA/Schema-owner controlled object creation — stronger security and accountability

Add a final highlighted takeaway box:

“KEY TAKEAWAY:
Access to existing objects in a common schema = YES.
Individual users freely creating objects only inside that common schema = NOT directly restrictable through a simple Oracle schema-level CREATE privilege.
Alternatives = Shared schema account, controlled DBA/schema-owner creation, or individual schemas.”

Design requirements:
- Professional Oracle DBA / enterprise architecture style
- White background
- Green = supported/recommended
- Red = restricted/security concern
- Amber = alternative/trade-off
- Clear arrows and database/schema icons
- Minimal text but enough technical detail
- Make it understandable to both technical managers and DBAs
- Landscape 16:9 layout suitable for Teams screen sharing or PowerPoint
- Avoid decorative graphics; prioritize architecture and decision-flow clarity
- Ensure all text is spelled correctly and readable