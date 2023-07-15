Validation rules verify that data entered by users in records meets the standards you specify before they can save it. 

You can create Validation rules for [[SObjects & Objects|objects]], fields, campaign members, or case milestones. 

A validation rule can contain a formula or expression that evaluates the data in one or more fields and returns a value of “`True`” or “`False`” which if `True` displays an error message to the user trying to create/modify a record. 

## How Salesforce Processes Validation Rules

Salesforce processes rules in the following order:
1. Validation rules
2. Assignment rules
3. Auto-response rules
4. Workflow rules (with immediate actions)
5. Escalation rules

## Validation Rule Field Restrictions

Validation rule formulas don’t or can’t refer to:

- Compound fields, including addresses, first and last names, and dependent picklists and lookups
- Campaign statistic fields, including statistics for individual campaigns and campaign hierarchies
- Merge fields for auto-number or compound address fields, such as Mailing Address

In relation to other fields and functions in Salesforce, validation rules behave as follows:

- The detail page of a custom activity field doesn't list associated validation rules.
- Workflow rules and some processes can invalidate previously valid fields. Invalidation occurs because updates to records based on workflow rules and also on process scheduled actions don’t trigger validation rules.
- Process record updates on immediate actions do fire validation rules.
- You can’t create validation rules for relationship group members.
- You can use roll-up summary fields in validation rules because the fields don’t display on edit pages. Don’t use roll-up summary fields as the error location.