Data types for "seven-shapes" (first version, VERY DRAFTY)
==========================================================

Here are the entities we collect data for, and what we collect about
them.

Please see the [data formats](data-formats.md) for *how* we store the
entities.

General
-------

Unless marked with a '+', a field is optional.

Some fields are "time varying", marked 't.v.'  An example might be a
woman's name: before marriage, during marriage, after she changed her
mind :-)

Time-varying fields can only be *one* thing on a particular date.
Taking names again: No aliases ("He went by Jim, but his mother called
him JB, and his underworld pals called him The Knife.").  Pick one.

Ids
---

Every data-thing has a type and an Id (identifier).  Types are fixed
and described below.  Generally, regexp for Ids: [a-z][a-z0-9-_]+

Other data may refer to an entity by its type and Id.  By far the most
common will be references to people/singers, thusly: \person{<singer-id>}
('person' is the type)

Ids are likely to be used as sort keys, so ... name appropriately.

Singing minutes
---------------

The dominant data type in this system is a set of singing minutes
(short: minutes).

Its Id is of the form [a-z0-9-]+_yyyy-mm-dd, where the pre-date part
is the Id of the singing series of which this singing is a part.

At the heart of singing minutes is a list of one or more "events",
usually denoting a song that was sung/led.  Each such "event" will/may
include:

   +Tunebook: typically a tunebook Id
   +Number: in the usual way (using SH editorial markings?)
   Tune Name: I realize this is redundant, but such redundancy
     will help catch errors on input; and make the archive files
     easy to read "as they are".
   Leaders: either \person{<singer-id>} or 'John Smith'
   Remarks: another other remark, perhaps with special coding for IMO etc?

Other "events" that may be included are:

1.  A RECESS possibly with a textual comment
2.  A MEAL possibly with a textual comment
3.  A MEETING to conduct business (with textual comment)
4.  A PRAYER/DEVOTION (?) that was offered  (ToDo: should this be generalized, somehow?)

Pseudo "events" that may be included are:

4.  A PROLOGUE: text intended to be put at the beginning of any report of this singing
5.  An EPILOGUE: text intended to be put at the end of any report of this singing
6.  A COMMENT: text not to process (i.e. a "comment" like in programming)
7   REMARKS: text about whatever, at a point in the proceedings

All of these "events" should be listed in chronological order, if possible.

A set of minutes also includes "header" fields,

    +Id: as above
    +Type: minutes
    +Version: see below
    +Series: Id of the singing series (inferred from the Id)
    +Date: start date of the singing (inferred from the Id), yyyy-mm-dd format.
     (ToDo: editorial marks in the case of uncertainty???)
    Compilers: person(s) who compiled/submitted the singing minutes
    Sources: document(s), if any, from which the singing minutes were taken

A set of minutes has a implicit Name constructed from the Name of the
series plus the minutes' Date.  (Everything has to have a Name.)

(The place a singing happened comes from the singing-series info.)

Singing series
--------------

A "singing series" is the concept to cover things like "the Etowah
Spring singing" or "the Georgia State Convention".  Most all-day
singings are part of an ongoing series.

Every singing is in a series, perhaps the only singing in the series.

A series may have *zero* or more singings in it.  (Why zero?  You
might know a singing series existed, but you haven't yet found any
minutes for it.)

Besides its type and Id, a singing series has:

    +Name: (t.v.) What it is normally called
    Place: (t.v.) Where it is held (including GPS coords?)
    Remarks: whatever someone might want to write about it

Person
------

A person is anyone who is mentioned in the minutes for any reason.

The main reason for this "person" thing is so we can definitively tie
together mentions of a person, i.e. "this John Smith and that one are
the same person", or just as important "these two John Smiths are
completely different people".

An Id for a person will customarily take the form of <last>-<first>,
possibly with a -<disambiguator>.  So you might have a person Id of
mugwallader-albert, but smith-john-the-pretty-one.

The merit of this naming "custom" is that someone could make progress
with singing data without having the "real" person data (which may be
restricted).

Besides its type and Id, a person has:

    +Name: (t.v.) what the person is normally called (in singing circles)
    Gender: use somebody else's list of categories :-)
    Address: (t.v.) town and state (plus non-US equivs); never more specific than that
    Bio: I would like for a person to be able to submit a (singing?)
      biography of his/her own, or perhaps for a deceased relative.
      It would be always understood to be *completely public*.

For those who have died, I'd like to do

    BirthName:
    BirthDate:
    BirthPlace:
    DeathDate:
    DeathPlace:
    BurialPlace:

As for relationships/children (so we can do singing family trees), I'm
not sure what to do.

Tunebook
--------

A tunebook entity describes a, well, tunebook.  The Id will usually be
a well-recognized abbreviation, e.g. SH for Sacred Harp.

A tunebook can be a general thing or very specific (depending on what
the minutes compiler wants to convey).  For instance, CHw might mean
"the Carolina book" (hey, all the variants are very very similar) and
CHw2015 might mean "the Folk Heritage Edition of the Carolina book
that came out in 2015".

Besides its type and Id, a tunebook has:

    +Name:
    Remarks: whatever anyone wants to write about it.

COMMON - Ids
------------

Every entity has an Id; we mentioned them earlier.

Ids must be unique within a type.  (Type, Id) pairs are unique within
a repository.  (It is *imaginable* that there might be multiple
repositories of singing data, and we might want to assign Repository
Ids, so that (RepositoryId, Type, Id) triples are globally unique.)

COMMON - Version
----------------

Every entity records what version of its data type it claims to
conform to.  A version number will change when it renders some of the
existing data "wrong".  Upward-compatible changes don't need a version
number change.

COMMON - Dates
--------------

Always yyyy-mm-dd (ISO 8601, folks).

We may need to expand the date format to be able to say
"sometime in June, not sure when", or "1962 or 1963, probably".

COMMON - text format
--------------------

There are many places where "text" is called for.  The current reality
is that will need to be turned into HTML and (somehow) PDF and, in the
future, no doubt something else.  That means there is going to be
"markup" of *some* kind.

We should perhaps subcontract this "little language" to a Markdown
variant, or something.

COMMON - Tags
-------------

I haven't put anything in yet for "tags", but I like 'em.  Any entity
could have one or more tags, each of which would mark some property of
the entity.

I favor *fixed* tag possibilities with defined meanings.  So, say, a
singing series might be tagged as "closed", where that is a defined
tag.  Tags should not be arbitrary strings that people make up.

COMMON - references
-------------------

We have previously mentioned that you can refer to a person as
\person{their-id}.  Similarly, you can have \minutes{minutes-id},
\series{series-id}, or \tunebook{book-id}.  Etc.

In text, these expand to the entity's Name (at the appropriate date).

COMMON - confidence marks
-------------------------

There are going to be times when you record info that makes you say
"This can't be right", "I can't tell if its a 3 or a 5", "There is no
such song as 799b", etc.  This will presumably arise more with
extracting minutes from historical documents.

There should be some general way to mark "red/yellow/green", i.e.
"red: serious doubts if this is right", "yellow: plausible but not
sure", "green: looks good to me!".

Two people might reach different conclusions.

ToDo: not sure how best to encode this.  A possibility might be:
\red{person-id}, \yellow{person-id}, \green{person-id} where the Id is
of the person who made the judgment.  I can imagine a common idiom
might be \yellow{partain-will}\footnote{7} (next section...), where
the footnote is then used to offer explanation.

COMMON - footnotes
------------------

I am wondering about entities all having "footnotes".  At basically
any place in any entity, you could write \footnote{2}, and then later
on have a field

    Footnote: 2. Explain whatever complicated thing needs explaining.

The footnote thing gives us a way to say "this bit of discussion/text
goes right here".

Not sure...

COMMON - optionality
--------------------

It's worth emphasizing that the stuff that minutes-secretaries might
bump into should not be onerous or picky.  You *might* record...

   CHw2015, 136, Sherburne, \person{partain-will}\yellow{smith-john}\footnote{7}
   FOOTNOTE, 7, \person{partain-will} was not alive on that date.

... which is kinda complicated!  Plain old ...

   CHw2015, 136, Sherburne, Will Partain

is still completely fine.

COMMON - media
--------------

(ToDo) It seems inevitable that we'll want to all this textual
information to audio/video/photographic bits, e.g. a photograph of a
person, or a scan of the newspaper from which the minutes were
extracted, or the audio of a singing, etc.  I don't have a great idea
how this should/might work...
