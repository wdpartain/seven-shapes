Data formats for "seven-shapes" (first version, VERY DRAFTY)
============================================================

Please see the [data types](data-types.md) for *what* entities we are
trying to record and store.

We want to store our data in a way that is *very* long-lived.  That
means simple and not tool-dependent.  That means "plain text", UTF-8
encoded.

The entity (type, Id) should be stored in folder 'type' and in a file
'Id.<extension>'.  (The 'type' and 'Id' are redundantly encoded in the
file, not just in the pathname, so this is not an utterly unbreakable
rule.)

All files shall be under version control (e.g. SVN/Git/whatever).

Non-minutes files
-----------------

A non-minutes entity (e.g. person, series, ...) is stored under a
pathname `type/Id.txt' with lines that are basically key-value pairs,
e.g.

    =%Id: partain-will
    %%Type: person
    %%Name: Will Partain

A value continues 'til the next key marker, e.g.

    =%Id: partain-will
    %%Type: person
    %%Name: Will
    Partain
    the
    old
    guy.

Leading and trailing whitespace is removed/doesn't count.

ToDo: how to encode time-varying fields.

Minutes files
-------------

A minutes entity (these are the overwhelmingly large part?) is stored
under a pathname `type/Id.csv' as -- ta-dah! -- a normal
comma-separated-values file; or... the simplest spreadsheet
imaginable.

Rows represent header fields or "events".  The header fields must come
first.  A keyword in column 1 says what kind of header field it is,
and subsequent columns deliver the goods (whatever they are).

The "events" (including pseudo-"events") are given, one per row, in
the order they occur.  A pseudo-"event" will have a keyword in column
one.  Otherwise, it will be a book/song/title/comment row (by far the
most common kind).

`*.csv` files must work with standard CSV libraries for major
programming languages.
