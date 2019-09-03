# vim 256 colors

```bash
$ cd ~/
$ mkdir .vim
$ cd .vim
$ mkdir colors
$ cd colors
$ curl -O https://raw.githubusercontent.com/morhetz/gruvbox/master/colors/gruvbox.vim
$ cd ~/
$ vim .vimrc
```
edit .vimrc 
```bash
vi ~/.vimrc
```
paste this to .vimrc

```bash
set number
syntax enable
set background=dark
colorscheme gruvbox
set mouse=a

if &term =~ '256color'
  " disable Background Color Erase (BCE) so that color schemes
  " render properly when inside 256-color tmux and GNU screen.
  " see also http://snk.tuxfamily.org/log/vim-256color-bce.html
  set t_ut=
endif

```