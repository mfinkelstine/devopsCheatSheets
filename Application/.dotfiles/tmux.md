# PuTTY + powerline
[Powerline](!https://github.com/powerline/powerline) or [Powerline font](!https://github.com/powerline/fonts) is a really nice looking command prompt and vim status bar.  When working on my linux machine, it was very easy to install following the instructions.

Trying to get it to work in Windows and PuTTY, however, took a bit of sleuthing.  To add onto the complexity, the project has changed, and some of the information that is out there is outdated.

So, here is what I did to make it work:
1. Follow the installation instructions for powerline
2. Download and install the DejaVu font in Windows.  Some of the other fonts in the site worked, and others didn't.  PuTTY seems to be picky on what fonts it will use, and the DejaVu font is a nice looking mono-spaced font, so it is a good starting point.
3. (Re)run PuTTY and create a new session with the following settings
    1. Window-Appearance-Font = DejaVu Sans Mono for Powerline
    2. Window-Appearance-Font Quality = ClearType 
    3. Window-Translation-Character Set = UTF-8
4. Verify your linux locale LANG=en_US.UTF-8  (mine was out of the box)
5. Verify that your .vimrc has "set encoding=utf-8"
6. Verify your term session is capable of 256 colors (TERM=xterm-256color)
7. And that's it!  I hope this helps.