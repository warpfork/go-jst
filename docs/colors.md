JST colorization
================

JST is primarily made to operate on text -- it reads and writes strings.

JST emits plain, uncolored text by default.
However, you can also spice it up a bit... as long as you have some for of markup for colors that we can embed in the strings we write out.

Fortunately, there's several kinds of markup which fit the bill:

1. terminal ANSI escape codes -- see https://en.wikipedia.org/wiki/ANSI_escape_code#3/4_bit
2. HTML -- you can probably find your own docs for this ;)

Be aware that if you configure JST to insert color markups, the output probably won't parse as JSON anymore!
(If you're building a program API, be sure you leave your users some way to turn colorizaton of output back _off_ again.)
