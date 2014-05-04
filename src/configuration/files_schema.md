# Configuration Files Schema

Monkey configuration files are unique on it's schema, they have been designed to force the user to keep a simple and readable format, if for some reason some of the Schema rules are broken, the Server will not start up and will raise an error.

HTTP Server configuration files tends to grow with the time when new [Virtual Hosts](../virtualhosts/README.md) are added or just because some rules are added. It's not a surprise to find __spaghetti__ configuration files when playing with well known HTTP servers.

Monkey implements three basic concepts that aims to address the __spaghetti__ problem:

* Sections
* Entries: Key/Value
* Indented Configuration Mode

A simple example of a configuration file is as follows:

```Python
[SERVER]
    # This is a commented line
    Port      2001
    KeepAlive on
```

## Sections

A section is defined by a name or title inside brackets. Looking at the example above a Server section have been set using __[SERVER]__ definition. Section rules:

* All section content must be indented.
* Multiples sections can exists on the same file.
* Under a section is expected to have comments and entries, a section cannot be empty.
* Any commented line under a section, must be indented too.

## Entries: Key/Value

A section may contain __Entries__, an entry is defined by a line of text that contains a __Key__ and a __Value__, using the above example, the __[SERVER]__ section contains two entries, one is the key __Port__ with value __2001__ and the other the key __KeepAlive__ with the value __on__. Entries rules:

* An entry is defined by a key and a value.
* A key must be indented.
* A key must contain a value which ends in the breakline.
* Multiple keys with the same name can exists.

Also commented lines are set prefixing the __#__ character, those lines are not processed but they must be indented too.

## Indented Configuration Mode

Monkey configuration files are based in a strict __Indented Mode__,  that means that each configuration file must follow the same pattern of alignment from left to right when writing text. Monkey by default suggest an indentation level of four spaces from left to right. Example:

```Python
[FIRST_SECTION]
    # This is a commented line
    Key1  some value

    Key2  another value

    # more comments

[SECOND_SECTION]
    KeyN  3.14
```

As you can see there are two sections with multiple entries and comments, note also that empty lines are allowed and they do not need to be indented.
