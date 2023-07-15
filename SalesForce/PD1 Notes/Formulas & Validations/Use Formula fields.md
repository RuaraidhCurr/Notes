When an org has a lot of data and your users need to access said data without doing a bunch of calculations in their head, Use the Formula field.

Let’s look at a specific example. What if you wanted to calculate how many days are left until an opportunity’s close date? You can create a simple formula field that automatically calculates that value. By adding the value to the Opportunity page layout, your users can quickly access this key information.

## Formula Data types
There are a number of different formula data types:

| DATA TYPE     | DESCRIPTION                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Checkbox**  | [](https://help.salesforce.com/s?language=en_US)Returns a true or false value. The field appears as a checkbox in record detail pages and reports. Use `True` for checked values and `False` for unchecked values.                                                                                                                                                                                                                                                                                                                                                                                               |
| **Currency**  | Returns a number in currency format of up to 18 digits with a currency sign.<br><br>![Note](https://resources.help.salesforce.com/images/9999d773bec62031a7926ed9be8b18f9.png)<br><br>NOTE Salesforce uses the round-half-to-even tie-breaking rule for currency fields. For example, 23.5 becomes 24, 22.5 becomes 23, −22.5 becomes −23, and −23.5 becomes −24.                                                                                                                                                                                                                                                |
| **Date**      | Returns data that represents a day on the calendar. The current date can be acquired by calling the built-in function `TODAY()` in a formula. This data type isn’t available for custom summary formulas in reports.                                                                                                                                                                                                                                                                                                                                                                                             |
| **Date/Time** | Returns data that represents a moment in time. A date/time field includes the date and also the time of day including hour, minutes, and seconds. You can insert the current date and time in a formula using the `NOW()` function. This data type isn’t available for custom summary formulas in reports.                                                                                                                                                                                                                                                                                                       |
| **Number**    | Returns a positive or negative integer or decimal of up to 18 digits. Salesforce uses the round half up tie-breaking rule for numbers in formula fields. For example, 12.345 becomes 12.35 and −12.345 becomes −12.35.<br><br>![Note](https://resources.help.salesforce.com/images/9999d773bec62031a7926ed9be8b18f9.png)<br><br>NOTE A formula field of type Number can store more decimals than are defined. For more information, see "[Data type number field can store more decimal places than defined](https://help.salesforce.com/s/articleView?id=000336833&language=en_US&type=1 "HTML (New Window)")." |
| **Percent**   | Returns a number in percent format of up to 18 digits followed by a percent sign. Percent data is stored as a decimal divided by 100, which means that 90% is equal to 0.90.                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| **Text**      | Returns a string of up to 3900 characters. To display text in addition to the formula output, insert that text in quotes. Use the text data type for text, text area, URL, phone, email, address, and auto-number fields. [](https://help.salesforce.com/s?language=en_US)This data type isn’t available for custom summary formulas in reports.<br><br>![Note](https://resources.help.salesforce.com/images/9999d773bec62031a7926ed9be8b18f9.png)<br><br>NOTE Text area isn’t a supported data type.                                                                                                            |
| **Time**      | Returns data that represents a moment in time, without the date. A time field includes the time of day by hour, minutes, seconds, and milliseconds. You can insert the current time in a formula using the `TIMENOW()` function.<br><br>![Note](https://resources.help.salesforce.com/images/9999d773bec62031a7926ed9be8b18f9.png)<br><br>NOTE In formula expressions, use the international date format (ISO) for text arguments. For example, use TIMEVALUE("11:30:00.000") instead of TIMEVALUE("11:30 AM").                                                                                                  |


## Elements of a Formula

A formula can contain references to the values of fields, operators, functions, literal values, or other formulas.

Use any or all of these elements to build a formula.

| Element Name   | Description                                                                               |
| -------------- | ----------------------------------------------------------------------------------------- |
| Literal Value  | A text string or number you enter that is not calculated or changed                       |
| Find Reference | Reference the value of another customer or standard field                                 |
| Function       | A system-defined formula that can require input from you and returns a value or values    |
| Operator       | A symbol that specifies the type of calculation to perform or the order in which to do it |
| Comment        | An annotation within a formula that begins with a forward slash asterisk `/*` and followed by an asterisk `/`                                                                                          |

## Formula Operators and Functions
There are many Formula Operators and functions, some are used more than others. There are the following categories:- 
- [Math Operators](https://help.salesforce.com/s/articleView?id=sf.customize_functions.htm&type=5#math_operators)
- [Logical Operators](https://help.salesforce.com/s/articleView?id=sf.customize_functions.htm&type=5#legal_operators)
- [Text Operators](https://help.salesforce.com/s/articleView?id=sf.customize_functions.htm&type=5#text_operators)
- [Date and Time Functions](https://help.salesforce.com/s/articleView?id=sf.customize_functions.htm&type=5#date_and_time_functinons)
- [Logical Functions](https://help.salesforce.com/s/articleView?id=sf.customize_functions.htm&type=5#logical_functions)
- [Math Functions](https://help.salesforce.com/s/articleView?id=sf.customize_functions.htm&type=5#math_functions)
- [Text Functions](https://help.salesforce.com/s/articleView?id=sf.customize_functions.htm&type=5#text_functions)
- [Summary Functions](https://help.salesforce.com/s/articleView?id=sf.customize_functions.htm&type=5#summary_functions)
- [Advanced Functions](https://help.salesforce.com/s/articleView?id=sf.customize_functions.htm&type=5#advanced_functions)

Follow the links in these to view the large list of functions & Operators 

## Formula Field Limits and Restrictions 
Formula fields have these three limits: 
1. Character Limit - Formula fields can only have 3900 characters, including spaces, comments and returns.
2. Save Size Limit - Formula fields can exceed 4,000 bytes when saved.
3. Compile Size Limits - Formula fields can't exceed 15,000 bytes when complied. 

## Formula Best Practices 
1. Put Every Function on a Separate Line
```
IF(AND(ISBLANK(myDate_c),active_c=true),"Missing Date","Not Applicable")
```
```
```
[](https://help.salesforce.com/s?language=en_US)`IF( AND( ISBLANK(myDate_c), active_c=true ), "Missing Date", "Not Applicable" )`