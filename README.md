go-jst
======

JSON Tables (or JST or for short) is a serializer that emits plain ol' json, while simultaneously leaning in on the insignificant whitespace to nice, columnar alignment that's human-friendly and easy to skim.  It still parses as completely regular json, with any unmodified- off-the-shelf json parser.

JST -- **JS**ON **T**ables -- a json marshaller and unmarshaller for pretty printing and pleasant reading.


Let's see it!
-------------

This picture is a side-by-side comparison with [`jq`](https://stedolan.github.io/jq/) output:

// TODO img

A table-like format means much more information can fit into the same two-dimensional space.
The columnar layout makes it easier to compare the same field across multiple objects.

(`jq` is of course _freakin awesome_.  I consider it an inspiration.
But this visualization shows nicely how prioritizing compactness and alignment can produce a very different feel.)


How does it work?
-----------------

By default, JST is based entirely on shameless (but effective) heuristics.  It's also configurable.

(Okay, it *will* be configurable.  This is a hobby project -- it does what I need it to do, when I need it.  "PRs welcome.")

You can read more about the heuristics TODO:HERE.

You can read more about the configuration mechanisms TODO:HERE.


Are there caveats?
------------------

Of course!

Given that JST is "it's just json", we have limits: we can't be like a spreedsheet that truncates the rendering of a cell, but expands on click, etc; we just don't have those options.
In general the JST formatter is making a best effort, and is designed to play nice if it's fed data that's playing (reasonably) nice.
It's definitely not guaranteed to do a perfect thing for all data with zero configuration; I don't think that's even possible.

JST doesn't try to format things nicely in the face of arbitrary input.
At some point you'll have to configure things if the heuristics don't work for you.

If someone puts in a 400-character long string, JST will still align a whole column to that (not truncate it).

JST sort of generally assumes you have infinite horizontal screen space.
If you don't have enough room to fit the data, it probably won't look nice anymore --
this depends a lot on how your terminal handles soft wrapping (or if it's clever enough to scroll).
There's only so much that can be done when we're doing plain text presentation and you've got more data than the screen can hold.
(There's also support for kicking some "columns" to their own line (like sub-tables automatically do), but it requires configuration.)

JST is not meant for being pretty, not maximizing throughput.  It also can't stream.
(You can't *do* columnar layout until you've seen how big all the columns have to be,
which inherently requires two passes over the data: one to find the sizes, and another to do the aligned printing.)

Overall, I'd consider using this on things like formatting (config files, or test fixture data files, etc); or rapid prototyping UI; things like that.
Be judicious about putting it in a situation that's going to handle totally unbounded user input.


License
-------

SPDX-License-Identifier: Apache-2.0 OR MIT
