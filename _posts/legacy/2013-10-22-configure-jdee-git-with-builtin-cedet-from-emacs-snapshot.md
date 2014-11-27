锲子
====

最近升级了emacs-snapshot发现jde不能用了，由于之前JDEE还是使用的
较老的2.4.0.1, CEDET使用的1.1, 索性决定JDEE升级到git(2.4.2 snapshot),
CEDET直接用builtin(2.0)版本。

Build JDEE from SVN
===================

``` {.example}
git svn clone https://svn.code.sf.net/p/jdee/code/trunk/jdee
ant configure
```

编辑build.properties
--------------------

``` {.example}
config.time=2013/10/21 22\:58
elib.dir=
prefix.dir=/home/joshua/.emacs.d/site-lisp/jdee-git
build.bin.emacs=emacs
```

``` {.bash}
ant build
ant install
```

最后配置emacs配置文件
---------------------

``` {.commonlisp}
(add-to-load-path-with-subdirs "~/.emacs.d/site-lisp/jdee-git/lisp")
(autoload 'jde-mode "jde" "JDE mode" t)
```

{{{more}}}

Bugfix
======

悲剧的是发现不能进入JDE mode, 主要涉及CEDET的2个文件需要修改

``` {.commonlisp}
/usr/share/emacs/24.3.50/lisp/cedet/semantic/fw.el
262c262
< (defun semantic-alias-obsolete (oldfnalias newfn when)
---
> (defun semantic-alias-obsolete (oldfnalias newfn &optional when)
/usr/share/emacs/24.3.50/lisp/cedet/mode-local.el
448c448
< (defun make-obsolete-overload (old new when)
---
> (defun make-obsolete-overload (old new &optional when)
```
