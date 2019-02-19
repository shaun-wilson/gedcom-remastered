# GEDCOM Remastered
A project to "remaster" the PDF GEDCOM Standard's, and transform them into human-readable and computer-parsable text based formats.

The original documents are rife with problems, including logical errors, inconsistent and incorrect syntax, and missing definitions. The content of the source documents (ie PDF's) has been manually copied into text files. All of the above problems have then been corrected, resulting in human-readable and computer-parsable text files.

__This project does NOT correct or extend the data model of the particular GEDCOM Standard it remaster's.__
This functionality will be developed in another project.

The remaster of a standard is stored in a branch of this repository, with the name containing the version number. At this stage, only 5.5.1 has been remastered. Other standards may be remastered in the future. The readme page for each standard/branch will have further information regarding the status of the remaster.

Other projects will be released in the near future that will allow these remastered standards to be used within Python 3.7+.

## File Formatting
A particular file format/syntax has been developed for these remastered standards. It is based upon the syntax in the GEDCOM 5.5.1. The syntax will remain consistent across all remastered standards, which will allow a single program/framework to work with many different data models - even user created ones.

The examples below include references to regex strings that have been tested with Python 3.7. The descriptions below should be considered as "commentary" of the syntax. The regex patterns should be considered the actual syntax definition.

__NOTE: THE FORMAT IS STILL BEING REFINED, AND WILL NOT BE FINALISED UNTIL AFTER THE FIRST REMASTER IS COMPLETE, AND COMMUNITY COMMENTS HAVE BEEN REFERRED!__

### Structures
TBA
### Sub-Structures
TBA
### Primitives
    DATE_RANGE:= {Size=8:35}
    [{BEF} <DATE>|{AFT} <DATE>|{BET} <DATE> AND <DATE>]
    The date range differs from the date period in that the date range is an estimate that an event happened on a single date somewhere in the date range specified.
    Where:
    AFT = Event happened after the given date.
    BEF = Event happened before the given date.
    BET = Event happened some time between date 1 AND date 2.
A primitive consists of 1 or more lines, as detailed detailed below.
Only the header line is required.
The order of lines is strict, in order to maintain consistency and simplify parsing.
The order is Header, Optional Value Definitions, Description, and Terms.
#### Header
The first line of the example - the header line - specifies the primitives label, and the primitive's size constraints. This line is required.

The label may contain upper case letters, numbers, underscores, and colons.
The label definition is completed with the string ":= ". This also separates the label and the size definition.
All primitives must have a size definition.
The size definition is wrapped in curly braces.
The size definition begins with the string "Size=".
One or more digits specify the minimum size of the primitive's value. 0 is a valid size (required for the NULL primitive).
A colon separates the minimum and maximum size numbers.
One or more digits specify the maximum size of the primitive's value. This number must be greater than or equal to the minimum size.

The colon separator and the maximum size number are an optional pair. If they are not present, the minimum size is considered as giving the field a fixed size. Thus, "Size=4" is the equivalent to "Size=4:4", as both definitions will restrict the primitive's value to 4 characters long.

A primitive header line can be matched with the regex `^([A-Z0-9_:]+?):= {Size=(\d+|\d+:\d+)}$`
#### Optional Value Definitions
The optional value definitions - the second line of the example - specifies the values that are allowable for the primitive.
The optional value definition line must follow the header.
Only one optional value definition line is allowed per primitive.

It is not required for primitives to specify the optional value definitions line. If it is not specified, a default option is used that allows a single TEXT primitive. Thus, a primitive without any optional value definitions specified is presumed to have specified the line "[<TEXT>]".
  
##### List
The options are wrapped in square brackets, effectively making a "list" of options.
Each option is separated by a pipe. Because of this, pipes cannot be used as strings within options, as it would complicate parsing.

Only one list of options is allowed (ie, the list is taken to be the entire line). This means lists cannot be stacked, as doing so would complicate parsing. For example, the GEDCOM 5.5.1 specs for the AGE_AT_EVENT primitive specify (shown unformatted) `[ < | > | <NULL>][ YYy MMm DDDd|...`, which is __INVALID__ by these remastered standards. This can be overcome by creating new primitives that effectively break the first list into it's own primitive. Thus, the following is a more appropriate definition that is correct by these standards `[<AGE_INEQUALITY><YY>y <MM>y <DDD>y|...`

Additionally, by requiring that the list's square brackets appear at the start and end of the line, it allows for square bracket characters to be used as strings within the options themselves. This is a required feature, as for example, GEDCOM 5.5.1 requires the text "[B.C.]" to be used in some options.

A line of optional value definitions can be matched with the regex pattern `^\[(.+)\]$`
From this match, the return value can be split by the `|` character, thus returning each option definition (a.k.a. an "item").
##### Item
A particular syntax applies within an option definition, a.k.a. an "item" of the "list".

If the definition is to include a nested primitive, the sub-primitive's label must be wrapped in <> characters, eg `<DATE>`.
A nested primitive's label can be matched from within a definition by the regex pattern `<([^>]+)>`

If the definition is to include a tag reference, the tag's label must be wrapped in %% characters, eg `%BIRT%`.
A tag reference's label can be matched from within a definition by the regex pattern `{([^}]+)}`

If the definition is to include a term reference (refer below), the term's label must be wrapped in {} characters, eg `{ABT}`.
A term reference's label can be matched from within a definition by the regex pattern `%([^%]+)%`

All other text within a definition is presumed to be either simple formatting characters (spaces, commas, etc), or words that have a universally known meaning, eg ` AND `. Some primitives only specify simple words, such as GEDCOM 5.5.1's LANGUAGE_ID primitive that has options such as `English`.
Due to the aforementioned constraints, these remastered standards do not allow strings/words/labels to contain the following characters: <>%{}|
Thus, a string of characters can be matched from within a definition by the regex pattern `([^{<%]+)`

To iterate over the portions of an option definition (ie, using Python's re.findall() method), the aforementioned regex patterns can be combined into the new pattern `<([^>]+)>|{([^}]+)}|%([^%]+)%|([^{<%]+)`
#### Description
The description line(s) - the third line of the example - is used to define the scope of the primitive as used within the model.

A description line is not required.
A description must come after the Optional Value Definitions line, if it is present. Otherwise, it may follow the Header.
The description can be more than 1 line.
Description lines continue to accrue until either a term definition header is found, another primitive header is encountered, or the end of file is reached.
The description can contain any otherwise valid characters allowed by the remastered standards.
#### Terms
The terms section begins at the fourth line of the example, and continues until the end of the example. The terms section is used to give specific meaning to strings found within the Optional Value Definitions.

A terms section is not required.
However, if term references are present within the primitive's Optional Value Definitions, then a terms section should be provided to describe each of them.
Similarly, a terms section should not be used if there are no term references within the primitive's Optional Value Definitions.

A terms section must follow any description line(s).
Otherwise, it must follow the Optional Value Definitions line, if it is present.
Otherwise, it may follow the Header.

A terms section consists of multiple structured lines, as described below;

##### Terms Header
A header line is used to mark the beginning of the terms section. It is required to delineate the primitive's description section (if present) from the term definitions. This enables greater freedom for the contents of the primitive's description, as it will prevent them from being incorrectly match as term definitions.

The regex pattern to match a terms header line is `^Where:$`
##### Term Definitions
A term definition line, such as the fifth line of the example, provides a definition for a term label. 
The label and definition are separated by the characters " = ".
A term label may not contain the following characters, as these would interfere with the pattern matching of the primitive's Optional Value Definition line (refer that section). The characters are <>%{}|
A term's description can contain any otherwise valid characters allowed by the remastered standards.
Term definition lines continue to accrue until either another primitive header is encountered, or the end of file is reached.

The regex pattern to match the label and description of a term definition line is `^(.+) = (.+)$`
### Tags
    BIRT {BIRTH}:=
    The event of entering into life.
A tag consists of 2 of more lines, as detailed detailed below;
#### Header
The first line of the example - the header line - specifies the tag, and the "formal name" (ie, label) of the tag.

The tag and label may contain upper case letters, numbers and underscores.
The label may also contain hyphens.
The tag and the label are separated with a space.
The label is wrapped in curly braces.
The header definition is completed with the characters :=.

A tag header line can be matched with the regex pattern `^([A-Z0-9]+) {([A-Z0-9_\-]+)}:=$`
#### Description
The description line(s) - the second line of the example - is used to define the value of the tag within the model.

The description lines must follow a tag header.
At least 1 description line is required.
Description lines continue to accrue until another tag Header is encountered, or the end of file is reached.
The description can contain any otherwise valid characters allowed by the remastered standards.
