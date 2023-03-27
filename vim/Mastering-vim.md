# Installation

## Seting up on Linux

Linux machines com with Vim installed. Vim 8 introduces some much-needed optimizations, run the following code:

>     git clone https://github.com/vim/vim.git

>     cd vim/src

>     make

>     sudo make install

#### TIP

If you're running into issues as you're installing Vim, you might be missing some dependencies. If you're using Debian-based distribution, the following command should add common missing dependencies: 

>     $ sudo apt-get install make build-essential libncurses5-dev libncursesw5-dev --fix-missing

If you don't care for being on the cutting edge, you can also update Vim using a package manager of your choice. Different Linux distributions use different package managers;

| Distribution                        | Command to install the latest version of Vim|
|-------------------------------------|-------------------------------------|
| Debian-based (Debian, Ubuntu, Mint) | $ sudo apt-get update, $ sudo apt-get install vim-gtk |
| CentOs (and Fedora prior to Fedora 22 | $ sudo yum check-update, $ sudo yum install vim-enhanced |
| Fedora 22+ | $sudo dnf check-update, $ sudo dnf install vim-enhanced |
| Arch | $ sudo pacman -Syum $ sudo pacman -S gvim |
| FreeBSD | $ sudo pkg update, $ sudo pgk install vim |

#### TIP

Vim uses package names for different repositories. Package like *vim-gtk* on Debian-based distributions or *vim-enhanced* on CentOs come with more features enabled (like GUI support for instance).

You can start the editor by typing the following command:

>     vim

On modern systems, you can also start Vim by invoking *vi*. There are no reason to use Vi over Vim, unless, for some reason, you're unable to install the latter.

## Setting up on MacOs

MacOs comes prepackaged with Vim, but the version can be outdated. There are a few ways to install a fresh version of Vim. First, you can install Vim using Homebrew, a package manager for MacOs. Second, you can download a .dmg image of MacVim. This experience would be more familiar because Mac users are used to the visual interface.

### Using Homebrew

Homebrew are available on *https://brew.sh*, and, consist of a single line executed in the following Command line:

>     /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

#### TIP

If you don't have XCode installed (which is often a prerequisite for any kind of development-related activity on Mac), you'll get an Xcode installation pop-up.

This should take a while to run, but you'll have Homebrew installed by the end: a fantastic tool you can use to install a lot more than Vim!.

Let's install a new version of Vim now using the following command:

>     brew install vim

Homebrew will install all the necesarry dependencies too, so you won't have to worry about a thing.

If you already have Homebrew installed, and you have installed Vim in the past, the preceding command will produce an error. You may want to make sure you have the last version of Vim, though, so run the following command:

>     brew upgrade vim

You should now be ready to enjoy Vim; let's try opening it with the following command:

>     vim

### Downloading a .dmg image

Navigate to *https://github.com/macvim-dev/macvim/releases/latest* and download *MacvIM.dmg*. 


## Setting up on Windows

Windows provides two primary routes for using Vim: setting up *Cygwin* and providing a more Unix-like command-line experience, or installing **gVim--a graphical version of Vim**(which support working with *cmd.exe* on Windows). gVim feels slightly more at home on Windows, while Cygwin might feel more at home if you're used to the Unix shell.

### Unix-like experience with Cywin

Cygwin is a Unix-like environment and a command-line interface for Windows. It aims to bring powerfull Unix shell and supporting tools to a Windonws machine.

#### Installing Cygwin

To begin the installation process, navigate to *https://cygwin.com/install.html* and download either *setup-x86_64.exe* or *setup-x86.exe* depending on the version of Windows you're using(64 bit or 32 bit respectively).

### Visual Vim with gVim

Navigate to *www.vim.org/download.php#pc* in your browser and download an executable installer. 

#### Verifying and troubleshooting the installation

Regardless of the platform you used to install Vim, it's good to make sure that, with Vim, all the right features enabled. On a command line, run the following command:

>     vim --version

You will see the output, with a set of features having a *+* and a *-* in front of them.

If your Vim is compiled with Python 2 support(+python) instead of Python 3 support(-python3). To correct the issue, I'd have to either recompile Vim with +python3 enabled or find a package which distributes a compiled version of Vim with +python3 enabled.

If we wanted to recompile Vim 8.1 with Python 3 support on Linux, we would do the following:

>     git clone https://github.com/vim/vim.git

>     cd vim/src

>     ./configure --with-features=huge --enable-python3interp

>     make

>     sudo make install


## Configuring Vim with your .vimrc

Vim reads configuration from a *.vimrc* file. 

In Linux and MacOs, .vimrc is located in your *user* directory(the full path would be */home/<username>/.vimrc*). You can also find out your *user* directory by opening a Command Prompt and running the following command:

>     echo $HOME

Windows does not allow periods in file names, so the file is named *_vimrc*. It's usually located in *C:\Users\<username>\_vimrc*, but you can also locate it by opening the Windows Command Prompt and running the following command:

>     echo %USERPROFILE%

syntax on                 " Enable syntax highlighting.
filetype plugin indent on  " Enable file type based indentation.
set autoindent             " Respect indentation when starting a new line.
set expandtab              " Expand tabs to spaces. Essential in Python.
set tabstop=4              " Number of spaces tab is counted for.
set shiftwidth=4           " Number of spaces to use for autoindent.

set backspace=2            " Fix backspace behavior on most terminals.

colorscheme murphy         " Change a colorscheme.

Lines starting with a double quote " are comments and are ignored by Vim. These settings bring in some sensible defaults, like syntax highlighting and consistent indentation.

#### TIP

When working with Vim configuration, you can try things out before adding them to your .vimrc file to do that, type : followed by a command, for example, :set autoindent. If you ever want to know the value of a setting, add ? at the end of the comand: for example :set tabstop? will tell you the current *tabstop* value.

#### TIP

Vim 8 comes prepackaged with the following color themes: blue, darkblue, default, delek, desert, elflord, evening, industry, koehler, morning, murphy, pablo, peachpuff, ron, shine, slate, torte, zellner. You can try out a color theme by typing :colorscheme <name>. 

## Common operations(or how to exit Vim)

### Opening files

#### TIP

If you're using gVim--you can open a file by going into a **File** menu and choosing **Open**. Sometimes graphical interface is exactly what you need!


