# Install Neovim

## Install neovim on Mac OS

Install neovim via homebrew
> brew install neovim

Move to *.config* folder

> cd .config

Create the directory nvim

> mkdir nvim

Move to the nvim diretory
> cd nvim 

Create a link symbolic
> ls -s "(which nvim) /opt/homebrew/bin/vim"

Create some directories in '~/config/nvim'

> mkdir lua \
> mkdir after \
> mkdir plugin

Base configuration

> vim init.lua

Directory temp, in ~/.config/nvim/

> vim

Create alias

> alias vim="nvim"

Inside the file 

> :set backuskip

Horizontal split

> :split or :sp

Vertical split 

> :vsplit or :vs

Edit another file

> :e [file]

Edit new file and write their name file

> :enew \
> :w [file]

Another way of the step avobe

> :e [name new file] 

Move to the last of the words on read mode

> e

Move to the start of the words on read mode

> b

Move to one to one on read mode

> l

Change one character on read mode

> ri

Set nvim or vim file and set wild ignore

> :set wildignore

Move one line to up

> k

Move one line to down

> j

Remove the space where is located

> ˆD

Select words

> ve

Select the line

> ˆv

Select multiple lines and put text

> ctrl v \
> select lines to down or up \
> Put the text \
> Press esc

Copy

> y

Paste

> p

Insert a new line

> ko

Install neovim packer

> git clone --depth 1 https://github.com/wbthomason/packer.nvim\
~/.local/share/nvim/site/pack/packer/start/packer.nvim

Go to the '~/.config/nvim/lua/plugins.lua' on read mode and install the plugins

> :PackerInstall
