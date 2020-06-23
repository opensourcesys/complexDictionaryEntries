[NVDA]: https://github.com/nvaccess/nvda/
# NVDA Complex Dictionary Entries Add-on #

Copyright (C) Open Source Systems, Ltd., all rights reserved. Licensed under the Gnu Public Licens version 3.

This is an add-on for the [NVDA] screen reader.

When [NVDA] refactored its speech code around version 2019.3, it broke support for certain popular use cases of its speech dictionaries.
The speech dictionaries change the way words, phrases, and other strings of text are spoken by the speech synthesizer.
Because of the new speech code, certain kinds of replacements became impossible.

## Example of what stopped working

A very good description of the problem is in [issue #11289](https://github.com/nvaccess/nvda/issues/11289).

## Methodology and objective

Ultimately, I would like to come up with something that can be contributed to NVDA core.
All suggestions and ideas (or pull requests!) gratefully appreciated.

That means that this add-on is mainly a proof of concept. If it works and is finished before the [feature freeze](https://github.com/nvaccess/nvda/issues/11006) ends, it may never make it out of development phase and instead go straight to PR against core.

## See also

For anyone wishing to understand or comment upon the direction of this add-on, in particular developers, please see:

* [To do list](todo.md); and
* [Roadmap](roadmap.md).
