## An autocompletion daemon for the Go programming language

Gocode is a helper tool which is intended to be integraded with your source code editor, like vim and emacs. It provides several advanced capabilities, which currently includes:

 - Context-sensitive autocompletion

It is called *daemon*, because it uses client/server architecture for caching purposes. In particular, it makes autocompletions very fast. Typical autocompletion time with warm cache is 30ms, which is barely noticeable.

Also watch the [demo screencast](http://nsf.110mb.com/gocode-demo.swf).

![Gocode in vim](http://nosmileface.ru/images/gocode-screenshot.png)

### Setup

 1. First of all, you need to get the latest git version of the gocode source code: 
    
    `git clone git://github.com/nsf/gocode.git`

 2. Change the directory:

    `cd gocode`

 3. At this step, please make sure that your **$GOBIN** is available in your **$PATH**. By default **$GOBIN** points to **$GOROOT/bin**. This is important, because editors assume that **gocode** executable is available in one of the directories, specified by your **$PATH** environment variable. Usually you've done that already while installing the Go compiler suite.

    Do these steps only if you know why do you need them:

    `export GOBIN=$HOME/bin`

    `export PATH=$PATH:$HOME/bin`

 4. Then you need to build the gocode and install it (make sure you're using GNU make):

    `make install`

 5. Next steps are editor specific. See below.

### Vim setup

In order to install vim scripts, you need to fulfill the following steps:

 1. Install official Go vim scripts from **$GOROOT/misc/vim**. If you did that already, proceed to the step 2.

 2. Install gocode vim scripts. Usually it's enough to do the following:

    `cd vim && ./update.bash`

    **update.bash** script does the following:

		#!/usr/bin/env bash
		mkdir -p ~/.vim/{autoload,ftplugin}
		cp autoload/gocomplete.vim ~/.vim/autoload
		cp ftplugin/go.vim ~/.vim/ftplugin

 3. Make sure vim has filetype plugin enabled. Simply add that to your **.vimrc**:

    `filetype plugin on`

 4. Autocompletion should work now. Use `<C-x><C-o>` for autocompletion (omnifunc autocompletion). 

### Emacs setup

In order to install emacs script, you need to fulfill the following steps:

 1. Install [auto-complete-mode](http://www.emacswiki.org/emacs/AutoComplete)

 2. Copy **emacs/go-autocomplete.el** file from the gocode source distribution to a directory which is in your 'load-path' in emacs.

 3. Add these lines to your **.emacs**:

 		(require 'go-autocomplete)
		(require 'auto-complete-config)

### Options

You can change all available options using `gocode set` command. The config file uses .ini-like format and usually stored somewhere in **~/.config/gocode** directory.

`gocode set` lists all options and their values.

`gocode set <option>` shows the value of that *option*.

`gocode set <option> <value>` sets the new *value* for that *option*.

 - *propose-builtins*

   A boolean option. If **true**, gocode will add built-in types, functions and constants to an autocompletion proposals. Default: **false**.

 - *lib-path*

   A string option. Allows you to override default location of the standard Go library. By default, it uses **$GOROOT/pkg/$GOOS_$GOARCH** in terms of previously existed environment variables. Also you can specify multiple paths using ':' (colon) as a separator.

### Debugging

If something went wrong, the first thing you may want to do is manually start the gocode daemon in a separate terminal window. It will show you all the stack traces and panics if any. Shutdown the daemon if it was already started and run a new one explicitly:

`gocode close`

`gocode -s`

Please, report bugs, feature suggestions and other rants to the [github issue tracker](http://github.com/nsf/gocode/issues) of this project.

### Developing

If you want to integrate gocode in your editor, please, contact me and I will tell you exactly what do you need. You can send me a message via github or simply contact me via <a href="mailto:no.smile.face@gmail.com">email</a>.

### Misc

 - It's a good idea to use the latest git version always. I'm trying to keep it in a working state.
 - Gocode always requires the latest Go compiler suite version of the 'release' or 'weekly' branch. Use tags to see what version is required. For example tag compatible-with-go-weekly-XXXX-XX-XX says that you can use Go version weekly.XXXX-XX-XX. Use `git log --decorate` to see tags in the git log. Each commit that was commited after compatible tag requires the same version as specified by this tag. If commit has multiple version tags, the use of the latest version amongst these is strongly recommended.
