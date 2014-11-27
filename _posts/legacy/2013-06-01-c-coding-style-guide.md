---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}

Tab or Spaces[^1]?
==================

From the <http://www.jwz.org/doc/tabs-vs-spaces.html> . We prefer to use
spaces.

Vim configuration
-----------------

``` {.example}
"To interpret tab as an 'indent' command instead of an insert-a-tab command, do this:
set softtabstop=4
"To set the mod-N indentation used when you hit the tab key in vim
set shiftwidth=4
"To cause the TAB file-character to be displayed as mod-N in vim
set tabstop=4
"To cause TAB characters to not be used in the file for compression, and for only spaces to be used
set expandtab
```

Emacs configuration
-------------------

``` {.example}
;;To set the mod-N indentation used when you hit the tab key in vim                                          
(setq c-basic-offset 4)
;;To cause the TAB file-character to be displayed as mod-N in vim                                            
(setq tab-width 4)
;;To cause TAB characters to not be used in the file for compression, and for only spaces to be used         
(setq indent-tabs-mode nil)
```

Astyle Configuaration[^2]
-------------------------

``` {.example}
#Indent using # spaces per indent
--indent=spaces=4 / -s4
```

{{{more}}}

Bracket
=======

Astyle Configuration
--------------------

``` {.example}
--brackets=linux / -l
```

Break brackets from namespace, class, and function definitions, but
attach brackets to statements within a function ( e.g. K&R / Linux style
). For example:

``` {.c}
void Foo(bool isFoo)
{
    if (isFoo) {
        bar();
    } else {
        anotherBar;
    }
}
```

Indentation Options
===================

Astyle Configuration
--------------------

``` {.example}
--indent-switches / -S
```

Indent 'switch' blocks so that the 'case X:' statements are indented in
the switch block. The entire case block is indented. For example:

``` {.c}
switch (foo)
{
    case 1:
        a += 1;
        break;

    case 2:
    {
        a += 2;
        break;
    }
}
```

``` {.example}
--indent-cases / -K
```

Indent 'case X:' blocks from the 'case X:' headers. Case statements not
enclosed in blocks are NOT indented. For example:

``` {.c}
switch (foo)
{
    case 1:
        a += 1;
        break;

    case 2:
        {
            a += 2;
            break;
        }
}
```

``` {.example}
--indent-preprocessor / -w
```

Indent multi-line preprocessor definitions ending with a backslash.
Should be used with --convert-tabs for proper results. For example:

``` {.c}
#define Is_Bar(arg,a,b) \
     (Is_Foo((arg), (a)) \
      |   | Is_Foo((arg), (b))) |
```

``` {.example}
--indent-col1-comments / -Y
```

This option will allow the comments to be indented with the code. For
example:

``` {.c}
void Foo()
{
    // comment
    if (isFoo)
        bar();
}
```

``` {.example}
--min-conditional-indent=0 OR -m0
```

Set the minimal indent that is added when a header is built of
multiple-lines. For example:

``` {.c}
// setting makes this non-bracketed code less clear
if (a < b
    |   | c > d) { |
    foo++;
}
```

Padding Options
===============

Astyle configuration
--------------------

``` {.example}
--pad-oper / -p
```

Insert space padding around operators. For example:

``` {.c}
if (foo == 2)
     a = bar((b - c) * a, * d--);
```

``` {.example}
--pad-header / -H
```

Insert space padding after paren headers only (e.g. 'if', 'for',
'while'...). Any end of line comments will remain in the original
column, if possible. For example:

``` {.c}
if (isFoo(a, b))
    bar(a, b);
```

``` {.example}
--unpad-paren / -U 
```

Remove extra space padding around parenthesis on the inside and outside.
For example:

``` {.c}
if ( isFoo( a, b ) )
    bar ( a, b );
```

becomes (with no padding option requested):

``` {.c}
if (isFoo(a, b))
    bar(a, b);
```

Formatting Options
==================

``` {.example}
--add-brackets / -j 
```

Add brackets to unbracketed one line conditional statements. For
example:

``` {.c}
if (isFoo)
    isFoo = false;
```

becomes:

``` {.c}
if (isFoo) {
    isFoo = false;
}
```

``` {.example}
--align-pointer=name   / -k3
```

Attach a pointer or reference operator (\* or &) to variable name
(right). For example:

``` {.c}
char &foo3;
int  *bar;
```

``` {.example}
--mode=c
--mode=cs
--mode=java
```

Indent a C/C++, C\#, or Java file. The option is usually set from the
file extension for each file. You can override the setting with this
entry. It will be used for all files regardless of the file extension.
It allows the formatter to identify language specific syntax such as C++
classes, templates, and keywords.

``` {.example}
--lineend=windows / -z1
--lineend=linux   / -z2
--lineend=macold  / -z3
```

Force use of the specified line end style. Valid options are windows
(CRLF), linux (LF), and macold (CR). MacOld style is the format for OS 9
and earlier. Mac OS X uses the Linux style. If one of these options is
not used the line ends will be determined automatically from the input
file.

Naming
======

We follow the linux naming style[^3], variable and method identifiers in
lowercase words seperated by underscores.

Local variable
--------------

Local variable should be short, and to the point. If you have some
random integer loop counter, it should probably be called "i".

Global variable
---------------

Do not use it. Please use descriptive name if you really need it.

Function/Method name
--------------------

Functions/Methods should be short and sweet, and do just one thing. The
function/method name should be descriptive as well. For example:

Enums
-----

Enum members and constants in ALL~CAPS~, words seperated by underscores.

Class/Struct
------------

Type identifiers in CamelCase.

Source files name.
------------------

Source files all lowercase, with no separators between words, in the
format `namespacefilename.c`.

Copyright header
================

Header at top of file contains:

``` {.c}
 /* Copyright (C) 2003-2010, Alpha Networks Co., LTD.                                                        
  *                                                                                                          
  * Author: Author Name <Author_Name@alphanetworks.com>                                                        
  * $Header$                                                                                                 
  * vim:ts=4:sw=4:
  * -*- mode: C; c-basic-offset: 4; tab-width: 4; indent-tabs-mode: nil -*-                                                                                   
  */

 /*                                                                                                          
  * The file description here.
  * ..
  */
```

Appendix
========

Command line using astyle
-------------------------

### Install astyle

-   Gentoo

``` {.example}
sudo emerge dev-util/astyle
```

-   Debian/Ubuntu

``` {.example}
sudo apt-get install astyle
```

### Using astlye to format the code

``` {.example}
astyle -s4lSKwYm0pHUjk3z2 test1.c test2.c 
```

Astyle configuration for vim
----------------------------

### Add following line into the `.vimrc`

``` {.example}
 """"""""""""""""""""""""""""""""""""""""""""""""""
 " tab setting
 """"""""""""""""""""""""""""""""""""""""""""""""""
 set tabstop=4
 set softtabstop=4
 set shiftwidth=4
 set expandtab
 """""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
 " Format the code with astyle
 """""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
 map <S-F> <Esc>:%! astyle -s4lSKwYm0pHUjk3z2<CR>
 autocmd BufNewFile,BufRead *.c set formatprg=astyle\ -s4lSKwYm0pHUjk3z2
```

### Format the current buffer with astyle

-   Format the whole buffer

press `Shift-F` Or press `gggqG`

-   Format the current line to the end of the buffer

press `gqG` = Format the next line press `gq+1` = Format current line
and next line press `gqj`

Astyle configuration for emacs
------------------------------

### Adding following line into the emacs configuration file

``` {.example}
;set c coding style
(eval-when-compile (require 'cc-defs))

;; Wrapper function needed for Emacs 21 and XEmacs (Emacs 22 offers the more
;; elegant solution of composing a list of lineup functions or quantities with
;; operators such as "add")
(defun alpha-c-lineup-expression-plus-4 (langelem)
  "Indents to the beginning of the current C expression plus 4 spaces."
  (save-excursion
    (back-to-indentation)
    ;; Go to beginning of *previous* line:
    (c-backward-syntactic-ws)
    (back-to-indentation)
    ;; We are making a reasonable assumption that if there is a control
    ;; structure to indent past, it has to be at the beginning of the line.
    (if (looking-at "\\(\\(if\\|for\\|while\\)\\s *(\\)")
        (goto-char (match-end 1)))
    (vector (+ 4 (current-column)))))

(defconst alpha-c-style
  `((c-recognize-knr-p . nil)
    (c-enable-xemacs-performance-kludge-p . t) ; speed up indentation in XEmacs
    (c-basic-offset . 4)
    (indent-tabs-mode . nil)
    (c-comment-only-line-offset . 0)
    (c-hanging-braces-alist . ((defun-open after)
                               (defun-close before after)
                               (class-open after)
                               (class-close before after)
                               (namespace-open after)
                               (inline-open after)
                               (inline-close before after)
                               (block-open after)
                               (block-close . c-snug-do-while)
                               (extern-lang-open after)
                               (extern-lang-close after)
                               (statement-case-open after)
                               (substatement-open after)))
    (c-hanging-colons-alist . ((case-label)
                               (label after)
                               (access-label after)
                               (member-init-intro before)
                               (inher-intro)))
    (c-hanging-semi&comma-criteria
     . (c-semi&comma-no-newlines-for-oneline-inliners
        c-semi&comma-inside-parenlist
        c-semi&comma-no-newlines-before-nonblanks))
    (c-indent-comments-syntactically-p . nil)
    (comment-column . 40)
    (c-cleanup-list . (brace-else-brace
                       brace-elseif-brace
                       brace-catch-brace
                       empty-defun-braces
                       defun-close-semi
                       list-close-comma
                       scope-operator))
    (c-offsets-alist . ((arglist-intro alpha-c-lineup-expression-plus-4)
                        (func-decl-cont . ++)
                        (member-init-intro . ++)
                        (inher-intro . ++)
                        (comment-intro . 0)
                        (arglist-close . c-lineup-arglist)
                        (topmost-intro . 0)
                        (block-open . 0)
                        (inline-open . 0)
                        (substatement-open . 0)
                        (statement-cont
                         .
                         (,(when (fboundp 'c-no-indent-after-java-annotations)
                             'c-no-indent-after-java-annotations)
                          ,(when (fboundp 'c-lineup-assignments)
                             'c-lineup-assignments)
                          ++))
                        (label . /)
                        (case-label . +)
                        (statement-case-open . +)
                        (statement-case-intro . +) ; case w/o {
                        (access-label . /)
                        (innamespace . 0))))
  "Alpha C/C++ Programming Style")

(defun alpha-set-c-style ()
  "Set the current buffer's c-style to Alpha C/C++ Programming
  Style. Meant to be added to `c-mode-common-hook'."
  (interactive)
  (make-local-variable 'c-tab-always-indent)
  (setq c-tab-always-indent t)
  (c-add-style "Alpha" alpha-c-style t))

(defun alpha-make-newline-indent ()
  "Sets up preferred newline behavior. Not set by default. Meant
  to be added to `c-mode-common-hook'."
  (interactive)
  (define-key c-mode-base-map "\C-m" 'newline-and-indent)
  (define-key c-mode-base-map [ret] 'newline-and-indent))

(defun astyle-this-buffer (pmin pmax)
  (interactive "r")
  (shell-command-on-region pmin pmax
                           "astyle -s4lSKwYm0pHUjk3z2" ;; add options here...
                           (current-buffer) t 
                           (get-buffer-create "*Astyle Errors*") t))

(add-hook 'c-mode-common-hook 'alpha-set-c-style)
(add-hook 'c-mode-common-hook 'alpha-make-newline-indent)
```

[^1]: <http://www.jwz.org/doc/tabs-vs-spaces.html>

[^2]: <http://astyle.sourceforge.net/astyle.html>

[^3]: <http://lxr.linux.no/linux/Documentation/CodingStyle>

    {{{more}}}

