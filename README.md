# GEDCOM 5.5.1 Remastered
This is the remaster of the GEDCOM Standard 5.5.1 Draft PDF dated 2 October 1999. This remaster is still a work in progress.

## Structural Changes
The following "structural" changes have been made to this standard. Note that these changes do NOT affect the data model.

### Structures
#### EVENT_DETAIL (modified)
Refer to the notes regarding "Custom EVEN TYPE" of INDIVIDUAL_EVENT_STRUCTURE.
#### FAMILY_EVENT_STRUCTURE (modified)
Refer to all the notes for INDIVIDUAL_EVENT_STRUCTURE.
#### INDIVIDUAL_ATTRIBUTE_STRUCTURE (modified)
Refer to the notes regarding "Custom EVEN TYPE" of INDIVIDUAL_EVENT_STRUCTURE.
#### INDIVIDUAL_EVENT_STRUCTURE (modified)
##### Asserted Value
Some definitions for this structure allowed for the value to be either <NULL> (which required the structure to have a sub-structure), or a "Y" flag (which indicated that the fact was asserted). The way the specification was written suggested that the null value could be used without a sub-structure, which is not a valid scenario.
  
To correct this, an ASSERT primitive was created to force the "Y" flag to be used. This also required extra definitions to be created, so that the ASSERT value can be used without a sub-structure, and vice-versa.
##### Custom EVEN TYPE
One of the defintions for this structure allowed for the EVEN tag to be used, in order to specify a custom event. However, it also required a row with the TYPE of event specified. The original definition had this sub-row contained within a sub-structure called EVENT_DETAIL, but the TYPE row was optional.

This was so that the structure EVENT_DETAIL could be used with or without a custom EVEN TYPE, but this means it does not properly validate all the scenarios that it used within.

Instead, the TYPE row was removed from the EVENT_DETAIL structure, and the TYPE row was added to the definitions that used EVENT_DETAIL. Additionally, the size field was modified to force the TYPE row when used as part of an EVEN row.
#### NOTE_STRUCTURE (modified)
The original specification allowed a <NULL> value, which suggested that the structure was valid without a pointer, text value, or sub-structures. This scenario is iteself not valid, and so the null option was removed.
#### SOURCE_REPOSITORY_CITATION (modified)
The original specification allowed for a <NULL> value to be used without any sub-structures, which is not valid. Instead, the definition was split into 2 distinct definitions; one that requires a pointer value but not sub-structures, and one that allows a null-value with a sub-structure.
  
### Primitives
#### AGE_AT_EVENT (modified)
The original standard had 2 "lists" of optional value definitions, which is not allowed by remaster syntax. To resolve this, the first list which contained the greater-than and less-than symbols was promoted to a new primitive called AGE_INEQUALITY.

Additionally, what should have been primitives representing the digits for years, months, and days, were place-holder strings YY, MM, and DDD. These strings were spun off to their own primitives: YEARS, MONTHS, MONTHS:IN_YEAR, DAYS, DAYS:IN_YEAR, and DAYS:IN_MONTH.
#### AGE_INEQUALITY (added)
Refer to the notes for AGE_AT_EVENT.
#### ASSERT (added)
Refer to the notes regarding "Asserted Value" of INDIVIDUAL_EVENT_STRUCTURE.
#### DATE (modified)
This primitive was changed to use the new DATE_CALENDAR_ESCAPED primitive. Refer to the notes for DATE_CALENDAR and DATE_CALENDAR_ESCAPE.
#### DATE_CALENDAR and DATE_CALENDAR_ESCAPE (removed)
The original standard was structured in such a way that there was no syntax linking a calendar date value and it's calendar escape code. Instead, it was inferred by the standards. In fact, the syntax as written would allow any escape code to be paired with any calendar date value, even if it is not logical to do so.

To correct this situation, these primitives were replaced by a new primitive called DATE_CALENDAR_ESCAPED. This primitive only allows the correct escape code to be paired with the proper calendar date value. It also allows for a Gregorian calendar date value to be used without an escape code, as the standards allow.
#### DATE_CALENDAR_ESCAPED (added)
Refer to the notes for DATE_CALENDAR and DATE_CALENDAR_ESCAPE.
#### DATE_EXACT (modified)
Refer to the notes for YEAR_GREG.
#### DATE_GREG (modified)
Refer to the notes for YEAR_GREG.
#### DAYS (added)
Refer to the notes for AGE_AT_EVENT.
#### DAYS:IN_MONTH (added)
Refer to the notes for AGE_AT_EVENT.
#### DAYS:IN_YEAR (added)
Refer to the notes for AGE_AT_EVENT.
#### GEDCOM_FORM (modified)
Refer to the notes for USER_DEFINED.
#### FRACSEC (added)
Refer to the notes for TIME_VALUE.
#### HOUR (added)
Refer to the notes for TIME_VALUE.
#### LDS_BAPTISM_DATE_STATUS, LDS_CHILD_SEALING_DATE_STATUS, LDS_ENDOWMENT_DATE_STATUS, LDS_SPOUSE_SEALING_DATE_STATUS (modified)
Some of these primitives had options specified that were not defined in the description, or descriptions of values that were not present in the options. These inconsistencies were corrected.
#### MINUTE (added)
Refer to the notes for TIME_VALUE.
#### NAME_TEXT (modified)
The standard imposed certain character restrictions on the text values of this primitive, but it's only option allowed regular unrestricted text, so the restrictions were not enforced. Instead, a specific text primitive was created to properly convey this information, and handle the restrictions. The new primitive is TEXT:NAME.
#### NAME_TYPE (modified)
Refer to the notes for USER_DEFINED.
#### NUMBER (modified)
The NUMBER primitive did not have a size constraint. One was added that forces a number to be at least 1 digit, and no longer than the maximum length of a line in the GEDCOM data file.
#### PHONETIC_TYPE (modified)
Refer to the notes for USER_DEFINED.
#### PLACE_TEXT (modified)
The standard imposed certain character restrictions on the text values of this primitive, but it's only option allowed regular unrestricted text, so the restrictions were not enforced. Instead, a specific text primitive was created to properly convey this information, and handle the restrictions. The new primitive is TEXT:PLACE.
#### ROMANIZED_TYPE (modified)
Refer to the notes for USER_DEFINED.
#### SECOND (added)
Refer to the notes for TIME_VALUE.
#### SEX_VALUE (modified)
Refer to the notes for USER_DEFINED.
#### TEXT:NAME (added)
Refer to the notes for NAME_TEXT. This primitive is intended to a sub-class of the TEXT primitive.
#### TEXT:PLACE (added)
Refer to the notes for PLACE_TEXT. This primitive is intended to a sub-class of the TEXT primitive.
#### TIME_VALUE (modified)
The option string for this primitive consisted of place-holder characters that were meant to represent units of time. Additionally, some of the place-holders were optional, and some of them were dependant upon the presence of other optional place-holders.

To correct this situation, the place-holders were promoted to proper primitives. These new primitives are HOUR, MINUTE, SECOND, and FRACSEC. Appropriate options were created to handle the valid scenarios.
#### USER_DEFINED (added)
Many primitives in the standard allowed for a "user defined" option, where the user could effectively enter any value, and it would be valid. This primitive was created to properly handle that user defined value, by making it an actual option.
#### YEAR_GREG (modified)
Refer to the notes for YEAR_GREG_BC. Note that this primitive was not actually modified, it is now just used as an option in less other primitives.
#### YEAR_GREG_BC (added)
The standard for YEAR_GREG states than an optional B.C. string can be added to the end of the year number, but did not show this in the options. To correct this situation, a new primitive was created, called YEAR_SUFFIX_BC.

However, this presented another problem, as some primitives that use YEAR_GREG showed a hard-coded B.C. suffix, and there was no indication that it was optional. Thus, YEAR_GREG_BC cannot be used in all places that YEAR_GREG can be, because it could result in the double-stacking of the B.C. suffix. For this reason, YEAR_GREG_BC was created.
#### YEAR_SUFFIX_BC (added)
Refer to the notes for YEAR_GREG_BC.
