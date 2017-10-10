
- - -
1. [Introduction](#introduction)  
2. [Installation](#installation)  
2.1. [Requirements](#requirements)  
2.2. [Installing syntastic with Pathogen](#installpathogen)  
3. [Recommended settings](#settings)  
4. [FAQ](#faq)  
4.1. [I installed syntastic but it isn't reporting any errors...](#faqinfo)  
4.2. [Syntastic supports several checkers for my filetype, how do I tell it which one(s) to use?](#faqcheckers)  
4.3. [How can I run checkers for "foreign" filetypes against the current file?](#faqforeign)  
4.4. [I have enabled multiple checkers for the current filetype. How can I display all errors from all checkers together?](#faqaggregate)  
4.5. [How can I pass additional arguments to a checker?](#faqargs)  
4.6. [I run a checker and the location list is not updated...](#faqloclist)  
4.6. [I run`:lopen` or `:lwindow` and the error window is empty...](#faqloclist)  
4.7. [How can I jump between the different errors without using the location list at the bottom of the window?](#faqlnext)  
4.8. [The error window is closed automatically when I `:quit` the current buffer but not when I `:bdelete` it?](#faqbdelete)  
4.9. [My favourite checker needs to load a configuration file from the project's root rather than the current directory...](#faqconfig)  
4.10. [What is the difference between syntax checkers and style checkers?](#faqstyle)  
4.11. [How can I check scripts written for different versions of Python?](#faqpython)  
4.12. [How can I check scripts written for different versions of Ruby?](#faqruby)  
4.13. [The `perl` checker has stopped working...](#faqperl)  
4.14. [What happened to the `rustc` checker?](#faqrust)  
4.15. [What happened to the `tsc` checker?](#faqtsc)  
4.16. [What happened to the `xcrun` checker?](#faqxcrun)  
5. [Resources](#otherresources)  

- - -

<a name="introduction"></a>

## 1\. Introduction

Syntastic is a syntax checking plugin for [Vim][vim] created by
[Martin Grenfell][scrooloose]. It runs files through external syntax checkers
and displays any resulting errors to the user. This can be done on demand, or
automatically as files are saved. If syntax errors are detected, the user is
notified and is happy because they didn't have to compile their code or execute
their script to find them.

At the time of this writing, syntastic has checking plugins for ACPI
Source Language, ActionScript, Ada, Ansible configurations, API Blueprint,
AppleScript, AsciiDoc, Assembly languages, BEMHTML, Bro, Bourne shell, C, C++,
C#, Cabal, Chef, CMake, CoffeeScript, Coco, Coq, CSS, Cucumber, CUDA, D, Dart,
DocBook, Dockerfile, Dust, Elixir, Erlang, eRuby, Fortran, Gentoo metadata,
GLSL, Go, Haml, Haskell, Haxe, Handlebars, HSS, HTML, Java, JavaScript, JSON,
JSX, Julia, LESS, Lex, Limbo, LISP, LLVM intermediate language, Lua, Markdown,
MATLAB, Mercury, NASM, Nix, Objective-C, Objective-C++, OCaml, Perl, Perl
6, Perl POD, PHP, gettext Portable Object, OS X and iOS property lists, Pug
(formerly Jade), Puppet, Python, QML, R, Racket, RDF TriG, RDF Turtle, Relax
NG, reStructuredText, RPM spec, Ruby, SASS/SCSS, Scala, Slim, SML, Solidity,
Sphinx, SQL, Stylus, Tcl, TeX, Texinfo, Twig, TypeScript, Vala, Verilog, VHDL,
Vim help, VimL, Vue.js, xHtml, XML, XSLT, XQuery, YACC, YAML, YANG data models,
z80, Zope page templates, and Zsh. See the [manual][checkers] for details about
the corresponding supported checkers (`:help syntastic-checkers` in Vim).

A number of third-party Vim plugins also provide checkers for syntastic, for
example: [merlin][merlin], [omnisharp-vim][omnisharp], [rust.vim][rust],
[syntastic-extras][myint], [syntastic-more][roktas], [tsuquyomi][tsuquyomi],
[vim-crystal][crystal], [vim-eastwood][eastwood], and [vim-swift][swift].

Below is a screenshot showing the methods that Syntastic uses to display syntax
errors. Note that, in practise, you will only have a subset of these methods
enabled.

![Screenshot 1][screenshot]

1. Errors are loaded into the location list for the corresponding window.
2. When the cursor is on a line containing an error, the error message is echoed in the command window.
3. Signs are placed beside lines with errors - note that warnings are displayed in a different color.
4. There is a configurable statusline flag you can include in your statusline config.
5. Hover the mouse over a line containing an error and the error message is displayed as a balloon.
6. (not shown) Highlighting errors with syntax highlighting. Erroneous parts of lines can be highlighted.

<a name="installation"></a>

## 2\. Installation

<a name="requirements"></a>

### 2.1\. Requirements

Syntastic itself has rather relaxed requirements: it doesn't have any external
dependencies, and it needs a version of [Vim][vim] compiled with a few common
features: `autocmd`, `eval`, `file_in_path`, `modify_fname`, `quickfix`,
`reltime`, `statusline`, and `user_commands`. Not all possible combinations of
features that include the ones above make equal sense on all operating systems,
but Vim version 7 or later with the "normal", "big", or "huge" feature sets
should be fine.

Syntastic should work with any modern plugin managers for Vim, such as
[NeoBundle][neobundle], [Pathogen][pathogen], [Vim-Addon-Manager][vam],
[Vim-Plug][plug], or [Vundle][vundle]. Instructions for installing syntastic
with [Pathogen][pathogen] are included below for completeness.

Starting with Vim version 7.4.1486 you can also load syntastic using the
standard mechanism of packages, without the help of third-party plugin managers
(see `:help packages` in Vim for details). Beware however that, while support
for packages has been added in Vim 7.4.1384, the functionality needed by
syntastic is present only in versions 7.4.1486 and later.

Last but not least: syntastic doesn't know how to do any syntax checks by
itself. In order to get meaningful results you need to install external
checkers corresponding to the types of files you use. Please consult the
[manual][checkers] (`:help syntastic-checkers` in Vim) for a list of supported
checkers.

<a name="installpathogen"></a>

### 2.2\. Installing syntastic with Pathogen

If you already have [Pathogen][pathogen] working then skip [Step 1](#step1) and go to
[Step 2](#step2).
