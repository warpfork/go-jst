JST configuration
=================

The configurability of `go-jst` is still somewhat limited, and you'll currently probably need to write code to insert the configuration.

Long run, I'd like to move towards declarative configuration systems -- more like config, less like code.
Eventually, we should be able to produce CLI tools that can be configured with no code at all.
But... there's some distance to go on this.


### color configuration

Check out the `jst.Color` type -- it's a field you can set in the `jst.Config` object, which in turn you can provide as an argument to the `MarshalConfigured` function.

See the [colors](colors.md) documentation for more info about color codes.

By default, JST output does no coloring.  You can configure it to use ANSI color codes by filling in this structure, though.

Currently it is only possible to highlight keys; in the future we want to support more kinds of highlighting.

We also want to support custom highlighting of specific keys and values -- this will be a future job for [structural configuration](#structural-configuration).


### structural configuration

We intend to use [IPLD Selectors](https://github.com/ipld/specs/tree/master/selectors) as the basis for powerful declarative configuration systems that can apply to any part of a document.

This could include...

- configuring some parts of a document to be table mode (or not);
- marking some columns to be pushed to their own line;
- adding color highlights to specific columns (or even specific values that are matched by an expression);
- and so on!

This is mostly future work.  Contributions welcome.
