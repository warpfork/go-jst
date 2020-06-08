JST structure detection
=======================

JST uses a set of simple heuristics to detect table-like data.
JST "strides" over the data once, looking for these patterns, and measuring the size that various parts of the data will take when printed;
then, it computes alignments that fit the whole data;
and finally, it proceeds to print.

There is also [configuration](configuration.md#structural-configuration) for specifying how data should be laid out, which may override these heuristics.


The heuristics
--------------

The following prose describes the rules JST follows to detect structure in the data.
These rules are applied in roughly the order documented here, and by following them in your mind,
you should be able to understand what JST is doing and how to structure data so that JST will work well on it.

Lists can be turned into column-aligned tables:

- Every time there's a list, and the first entry is a map, we'll try to treat it as a table.
- Every time a map in that list starts with the same first key as the first map did, it's a table row.
- Every thing that's a table row will be buffered, and we try to fit each key into a table column.
- (FUTURE) You can manually specify keys that should be excluded from columns; these will be shifted to the end and packed tightly.
- We'll store the size of the widest value for each column.  We'll need to do this over every row so we can align output spacing.
- If there's a map in the list that doesn't start with the same first key, it's a table exclusion, and gets formatted densely.
- If a map has a value that's a list, we attempt to apply this whole ruleset over again from the top.
- If a table is detected, we'll print the key on its own new line, slightly indented, together with the list open.  Then, emit the table on subsequent further indented lines.

Maps can also be turned into column-aligned tables:

- You have to use additional configuration to engage this: by default, only lists trigger table mode.
- The search for table rows begins anew with each map value.  The map keys form a defacto first column.
- Thereafter, all the rules for handling each table row is the same the rules described above for lists.


### demo

Consider this demonstration data:

```json
[
  {"path": "./foo",  "moduleName": "whiz.org/teamBar/foo", "status": "changed"},
  {"path": "./baz",  "moduleName": "whiz.org/teamBar/baz", "status": "green"},
  {"path": "./quxx", "moduleName": "example.net/quxx",     "status": "lit",
    "subtable": [
      {"widget": "shining",       "property": "neat", "familiarity": 14},
      {"widget": "shimmering",    "property": "neat", "familiarity": 140},
      {"widget": "scintillating",                     "familiarity": 0},
      {"widget": "irridescent",   "property": "yes"},
    ]}
]
```

This is JST formatted JSON.

Let's walk through the heuristics on this data to see how:

1. There's a list, and the first entry is a map!  We'll try to treat it as a table.
2. The first key of this first map is "path", so that'll be our hint that future entries are a table row.
3. "path", "moduleName", and "status" are all columns we've detected in this table by the first row.  (We'll start remembering the max size for each column as we procede through the rest of the data.)
4. When we look at the second list entry, it's a map, and its first entry is "path", so it's still a table row.  It doesn't have any new columns, and we update the max size known for each column (none are bigger, though).
5. When we look at the third list entry, it's a map, and it's first entry is "path", so it's _still_ a table row...
6. But what's this -- a new column, called "subtable"?  It contains a list!  So, we recurse up to the top of our heuristics... could this list _be a table_?
7. The first entry of this new list is a map!  It's a table!

... you get the idea.
