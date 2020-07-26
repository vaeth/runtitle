# runtitle

Scripts to run commands and set the hard status line (windows title)

(C) Martin Väth (martin at mvath.de).
This project is under the BSD license 2.0 (“3-clause BSD license”).
SPDX-License-Identifier: BSD-3-Clause

## Installation

For installation, copy the content of `bin/` in your `$PATH`.
To obtain support for __zsh completion__ (also for other programs using
`title`), you can copy the content of `zsh/` into a directory of your
zsh's `$fpath`.

For Gentoo, there is an ebuild in the mv overlay (available over layman).

## Description

This project contains two simple scripts:

- `runner`

   Runs a command, setting the title and ringing the bell if command fails.
   There are options to repeat the command (a number of times or if it fails).
   Use `runner -h` to obtain a description.

- `title`

  Sets the hard status line if the terminal supports it.
  Use `title -h` to obtain a description.

If you do not give a text to `title`, nothing is printed
(pass an empty text if you want to clear the status line).

You can source `title` into a POSIX shell with
```
TitleInit() {
. title "$@"
}
```
Then the function `TitleInit` can be used with the same meaning as `title`,
(but can be used repeatedly without restarting `title`).
Moreover, after `TitleInit` the function `Title` can be used (also repeatedly)
to set the title according to the options of `TitleInit`.
Also, there are functions `TitlePlain` and `TitleVerbose` which act
correspondingly to `title -p` or `title -P`, respectively.
If you only want to define `Title`, `TitlePlain`, `TitleVerbose`, and
`TitleInit` by sourcing, use option `-q` in the call to `TitleInit`.
The function `TitleInit` may override variable names of the form
`titleinit_`*`_`.

The following function `Title` will define `Title`, `TitlePlain`,
`TitleVerbose`, and `TitleInit` appropriately at its first call
(if `title` is in the `$PATH`) and then act as `Title`:
```
Title() {
Title() {
:
}
command -v title >/dev/null 2>&1 || return 0
TitleInit() {
. title "$@"
}
TitleInit [options to title] -- "$@"
}
```
In the file `bin/runner` you find an example how this can be combined with
setting a `trap` so that a sane title is "restored" at program exit.

If you write a program using `Title`, I recommend to equip this program
with an option with an argument which is passed to `TitleInit`, and where
the option argument `-` means that the title script is not used.
Since version 2.3 you can get a proper __zsh completion__ for such an option
for your program by using the provided autoloaded function `_title_opt`;
see `zsh/_runner` for a sample usage of that function.
