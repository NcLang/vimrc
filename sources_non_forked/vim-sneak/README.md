sneak.vim :shoe:
================

Sneak is a Vim plugin that jumps to any location specified by two characters. It
works with **multiple lines**, **operators** (including repeat `.` and
[surround]), **[keymaps]**, **visual** mode, **[unicode]** ("multibyte"), and
**macros**.

  - Preserves default `f`, `t`, `;` and `,` behavior
  - Repeats operation with `.`
  - Repeats motion with `;` and `,`
  - Jumps immediately to the first match
  - Updates the jumplist only for non-repeat motion
  - Restricts to a vertical scope if *count* is used
  - Provides *label-mode* for a minimalist
  [EasyMotion](https://github.com/Lokaltog/vim-easymotion) alternative: `let g:sneak#label = 1`
  - More at [FAQ](#faq)

Vim has many built-in motions, but "medium distance" is missing...

    l  f  t  %  'm  }  ]m  ]]  M  L         /
                                       ^
                                       |
                                     sneak

Usage (Default)
---------------

<a href="http://imgur.com/Jke0mIJ" title="Click to see a short demo"><img src="https://raw.github.com/justinmk/vim-sneak/fluff/assets/readme_diagram.png"></a>

Sneak is invoked with `s` followed by exactly two characters:

    s{char}{char}

* Press `sab` to **move the cursor** immediately to the next instance of the text "ab".
    * Additional matches, if any, are highlighted until the cursor is moved.
* Press `;` to go to the next match (or `s` again, if `s_next` is enabled; see [`:help sneak`](doc/sneak.txt)).
* Press `3;` to skip to the third match from the current position.
* Press `ctrl-o` or ``` `` ``` to go back to the starting point.
    * This is a built-in Vim motion; Sneak adds to Vim's [jumplist](http://vimdoc.sourceforge.net/htmldoc/motion.html#jumplist)
      *only* on `s` invocation—not repeats—so you can 
      abandon a trail of `;` or `,` by a single `ctrl-o` or ``` `` ```.
* Press `s<Enter>` at any time to repeat the last Sneak-search.
* Press `S` to search backwards.

Sneak can be **scoped** to a column of width 2×[number] by prefixing `s`
with a number.

* Press `5sxy` to go immediately to the next instance of "xy" within 5 columns
  of the cursor.
    * A highlight block indicates the vertical scope.

Sneak is invoked with [**operators**](http://vimdoc.sourceforge.net/htmldoc/motion.html#operator)
via `z` (because `s` is taken by surround.vim).

* Press `3dzqt` to delete up to the *third* instance of "qt".
    * Press `.` to repeat the `3dzqt` operation.
    * Press `2.` to repeat twice.
    * Press `d;` to delete up to the next match.
    * Press `4d;` to delete up to the *fourth* next match.
* Press `ysz))]` to surround in brackets up to `))`.
    * Press `;` to go to the next `))`.
* Press `gUz\}` to upper-case the text from the cursor until the next instance
  of the literal text `\}`
    * Press `.` to repeat the `gUz\}` operation.

Installation
------------

- [vim-plug](https://github.com/junegunn/vim-plug)
  - `Plug 'justinmk/vim-sneak'`
- [Pathogen](https://github.com/tpope/vim-pathogen)
  - `cd ~/.vim/bundle && git clone git://github.com/justinmk/vim-sneak.git`
- Manual installation:
  - Copy the files to your `.vim` directory.

To repeat Sneak *operations* (like `dzab`) with dot `.`,
[repeat.vim](https://github.com/tpope/vim-repeat) is required.

FAQ
---

#### Why not use `/`?

For the same reason that Vim has [motions](http://vimdoc.sourceforge.net/htmldoc/motion.html#left-right-motions)
like `f` and `t`: common operations should use as few keystrokes as possible.

* `/ab<cr>` requires 33% more keystrokes than `sab`
* sets *only* the initial position in the Vim jumplist—so you can explore a
  trail of matches via `;`, then return to the start with a single `ctrl-o` or ``` `` ```
* doesn't clutter your search history
* input is always literal (no need to escape special characters)
  * ignores accents ("equivalence class") when matching
    ([#183](https://github.com/justinmk/vim-sneak/issues/183))
* smarter, subtler highlighting
* sneak *label-mode*

#### Why not use `f`?

* Sneak is *fifty times* more precise than `f` or `t`
* Sneak moves vertically
* Sneak highlights matches in the direction of your search

#### How dare you remap `s`?

You can specify any mapping for Sneak (see [`:help sneak`](doc/sneak.txt)).
By the way: `cl` is equivalent to `s`, and `cc` is equivalent to `S`.

#### How can I replace `f` with Sneak?

```
    nmap f <Plug>Sneak_s
    nmap F <Plug>Sneak_S
    xmap f <Plug>Sneak_s
    xmap F <Plug>Sneak_S
    omap f <Plug>Sneak_s
    omap F <Plug>Sneak_S
```

#### How can I replace `f` and/or `t` with *one-character* Sneak?

Sneak provides `<Plug>` convenience-mappings for `f` and `t` 1-character-sneak.
These mappings do *not* invoke label-mode, even if you have it enabled.
```
    "replace 'f' with 1-char Sneak
    nmap f <Plug>Sneak_f
    nmap F <Plug>Sneak_F
    xmap f <Plug>Sneak_f
    xmap F <Plug>Sneak_F
    omap f <Plug>Sneak_f
    omap F <Plug>Sneak_F
    "replace 't' with 1-char Sneak
    nmap t <Plug>Sneak_t
    nmap T <Plug>Sneak_T
    xmap t <Plug>Sneak_t
    xmap T <Plug>Sneak_T
    omap t <Plug>Sneak_t
    omap T <Plug>Sneak_T
```

Related
-------

* [Seek](https://github.com/goldfeld/vim-seek)
* [EasyMotion](https://github.com/Lokaltog/vim-easymotion)
* [smalls](https://github.com/t9md/vim-smalls)
* [improvedft](https://github.com/chrisbra/improvedft)
* [clever-f](https://github.com/rhysd/clever-f.vim)
* [vim-extended-ft](https://github.com/svermeulen/vim-extended-ft)
* [Fanf,ingTastic;](https://github.com/dahu/vim-fanfingtastic)

License
-------

Copyright © Justin M. Keyes. Distributed under the MIT license.

[unicode]: http://vimdoc.sourceforge.net/htmldoc/mbyte.html#UTF-8
[keymaps]: http://vimdoc.sourceforge.net/htmldoc/mbyte.html#mbyte-keymap
[surround]: https://github.com/tpope/vim-surround
[count]: http://vimdoc.sourceforge.net/htmldoc/intro.html#[count]
