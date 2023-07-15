A roll-up summary calculate values from a set of related record.

You can create roll-up summary fields that automatically display a value on a master record based on the values of records in a detail record. These detail records must be directly related to the master through a master-detail relationship.

You can perform different types of calculations with roll-up summary fields:

|Type|Description|
|---|---|
|COUNT|Totals the number of related records.|
|SUM|Totals the values in the field you select in the Field to Aggregate option. Only number, currency, and percent fields are available.|
|MIN|Displays the lowest value of the field you select in the Field to Aggregate option for all directly related records. Only number, currency, percent, date, and date/time fields are available.|
|MAX|Displays the highest value of the field you select in the Field to Aggregate option for all directly related records. Only number, currency, percent, date, and date/time fields are available.|

You can create roll-up summary fields on:
- Any custom object on the master side of a master-detail relationship
- Any standard object on the master side of a master-detail relationship with a customer object
- Opportunities(Opp) using the value of the opp products related to the opp
- Account using the values related opp
- Campaigns using campaign member status or the values of a campaign member custom field

Sometimes you cannot change the field type of a field that you have called a roll-up summary field on. 

## Best Practices
- Apply field-level security to your roll-up summary fields if they calculate values that you don't want visible to users.
- Consider how validation rules may affect your roll-up summery fields
- You can use roll-up summery fields in validation rules but not as the error location for your validation.
- Advanced currency management affects roll-up summary fields
- When you refer to a roll-up summary field in a list view or report, you can’t use certain qualifiers, including:
    - Starts with
    - Contains
    - Does not contain
    - Includes
    - Excludes
    - Within