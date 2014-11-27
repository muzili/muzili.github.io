Why
===

ibus输入法没有一个比较好的词库, google pinyin for ibus
略好但是还是和Windows下有很大的差距. 因为现在的fcitx支持 sogou输入法,
所以我想试一下

How
===

{{{more}}}

安装fcitx
---------

> sudo aptitude install fcitx

Download sogou from `http://pinyin.sogou.com/linux/`
----------------------------------------------------

> dpkg -i sogou~pinyinlinux1~.0.0.0011~amd64~.deb

Diactivate the system default ibus
----------------------------------

> gsettings set org.gnome.settings-daemon.plugins.keyboard active false

Enable fcitx for en~US~ env
---------------------------

> sudo ln -sf /etc/X11/xinit/xinput.d/fcitx
> /etc/X11/xinit/xinput.d/en~US~
>
> sudo ln -sf /etc/alternatives/xinput-en~US~
> /etc/X11/xinit/xinput.d/en~US~

Remove the duplicated shortcut "Control + Space"
------------------------------------------------

### "Win" -\> Input "Shortcut" -\> select "keyboard" -\> select "Shortcut" / "Typing" -\> Disable "Switch to next input source" by press "Backspace" key

现在就可以尽情的享受sogou输入法了, 体验还不错.

