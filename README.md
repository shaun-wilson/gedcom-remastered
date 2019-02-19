# GEDCOM 5.5.1 Remastered
This is the remaster of the GEDCOM Standard 5.5.1 Draft PDF dated 2 October 1999. This remaster is still a work in progress.

## Structural Changes
The following "structural" changes have been made to this standard. Note that these changes do NOT affect the data model.

### Primitives
#### AGE_AT_EVENT (modified)
The original standard had 2 "lists" of optional value definitions, which is not allowed by remaster syntax. To resolve this, the first list which contained the greater-than and less-than symbols was promoted to a new primitive called AGE_INEQUALITY.

Additionally, what should have been primitives representing the digits for years, months, and days, were place-holder strings YY, MM, and DDD. These strings were spun off to their own primitives: YEARS, MONTHS, MONTHS:IN_YEAR, DAYS, DAYS:IN_YEAR, and DAYS:IN_MONTH.
#### AGE_INEQUALITY (added)
Refer to the notes for AGE_AT_EVENT.
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