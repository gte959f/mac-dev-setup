# Mac OS X Dev Setup

This document describes how I set up my developer environment on a new MacBook or iMac. We will set up [Node](http://nodejs.org/) (JavaScript), [Python](http://www.python.org/), and [Ruby](http://www.ruby-lang.org/) environments, mainly for JavaScript and Python development. Even if you don't program in all three, it is good to have them as many command-line tools use one of them. As you read  and follow these steps, feel free to send me any feedback or comments you may have.

The document assumes you are new to Mac. The steps below were tested on **OS X Mountain Lion**.

If you have any comments or suggestions, feel free to give me a shout [on Twitter](https://twitter.com/nicolahery)!

- [System update](#system-update)
- [System preferences](#system-preferences)
- [Google Chrome](#google-chrome)
- [iTerm2](#iterm2)
- [Homebrew](#homebrew)
- [Consolas](#consolas)
- [Beautiful terminal](#beautiful-terminal)
- [iTerm2](#iterm2)
- [Git](#git)
- [Sublime Text](#sublime-text)
- [Vim](#vim)
- [Python](#python)
- [Virtualenv](#virtualenv)
- [IPython](#ipython)
- [Numpy and Scipy](#numpy-and-scipy)
- [MySQL](#mysql)
- [Node.js](#nodejs)
- [Ruby and RVM](#ruby-and-rvm)
- [Heroku](#heroku)
- [MongoDB](#mongodb)
- [Redis](#redis)
- [VirtualBox](#virtualbox)
- [Vagrant](#vagrant)
- [Docker](#docker)
- [Amazon Tools](#amazon-tools)
- [Projects folder](#projects-folder)
- [Apps](#apps)

## System update

First thing you need to do, on any OS acutally, is update the system! For that: **Apple Icon > Software Update...**

## System preferences

If this is a new computer, there are a couple tweaks I like to make to the System Preferences. Feel free to follow these, or to ignore them, depending on your personal preferences.

In **Apple Icon > System Preferences**:

- Trackpad > Tap to click
- Keyboard > Key Repeat > Fast (all the way to the right)
- Keyboard > Delay Until Repeat > Short (all the way to the right)
- Dock > Automatically hide and show the Dock

## Google Chrome

Install your favorite browser, mine happens to be Chrome.

Download from [www.google.com/chrome](https://www.google.com/intl/en/chrome/browser/). Open the **.dmg** file once it's done downloading (this will mount the disk image), and drag and drop the **Google Chrome** app into the Applications folder (on the Mac, most applications are installed this way). When done, you can unmount the disk in Finder (the small "eject" icon next to the disk under **Devices**).

## iTerm2

Since we're going to be spending a lot of time in the command-line, let's install a better terminal than the default one. Download and install [iTerm2](http://www.iterm2.com/) (the newest version, even if it says "beta release").

In **Finder**, drag and drop the **iTerm** Application file into the **Applications** folder.

You can now launch iTerm, through the **Launchpad** for instance.

Let's just quickly change some preferences. In **iTerm > Preferences...**, under the tab **General**, uncheck **Confirm closing multiple sessions** and **Confirm "Quit iTerm2 (Cmd+Q)" command** under the section **Closing**.

In the tab **Profiles**, create a new one with the "+" icon, and rename it to your first name for example. Then, select **Other Actions... > Set as Default**. Finally, under the section **Window**, change the size to something better, like **Columns: 125** and **Rows: 35**.

When done, hit the red "X" in the upper left (saving is automatic in OS X preference panes). Close the window and open a new one to see the size change.

## Homebrew

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for OS X is [Homebrew](http://mxcl.github.com/homebrew/).

### Install

An important dependency before Homebrew can work is the **Command Line Tools** for **Xcode**. These include compilers that will allow you to build things from source.

Now, Xcode weights something like 2GB, and you don't need it unless you're developing iPhone or Mac apps. Good news is Apple provides a way to install only the Command Line Tools, without Xcode. To do this you need to go to [http://developer.apple.com/downloads](http://developer.apple.com/downloads), and sign in with your Apple ID (the same one you use for iTunes and app purchases). Unfortunately, you're greeted by a rather annoying questionnaire. All questions are required, so feel free to answer at random.

Once you reach the downloads page, search for "command line tools", and download the latest **Command Line Tools (OS X Mountain Lion) for Xcode**. Open the **.dmg** file once it's done downloading, and double-click on the **.mpkg** installer to launch the installation. When it's done, you can unmount the disk in Finder.

Finally, we can install Hombrew! In the terminal paste the following line (without the `$`), hit **Enter**, and follow the steps on the screen:

    $ ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"

One thing we need to do is tell the system to use programs installed by Hombrew (in `/usr/local/bin`) rather than the OS default if it exists. We do this by adding `/usr/local/bin` to your `$PATH` environment variable:

    $ echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile

Open an new terminal tab with **Cmd+T** (you should also close the old one), then run the following command to make sure everything works:

    $ brew doctor
    
### Usage

To install a package (or **Formula** in Homebrew vocabulary) simply type:

    $ brew install <formula>
        
To update Homebrew's directory of formulae, run:

    $ brew update
    
**Note**: I've seen that command fail sometimes because of a bug. If that ever happens, run the following (when you have Git installed):

    $ cd /usr/local
    $ git fetch origin
    $ git reset --hard origin/master

To see if any of your packages need to be updated:

    $ brew outdated
    
To update a package:

    $ brew upgrade <formula>
        
Homebrew keeps older versions of packages installed, in case you want to roll back. That rarely is necessary, so you can do some cleanup to get rid of those old versions:

    $ brew cleanup

To see what you have installed (with their version numbers):

    $ brew list --versions

## Consolas

I really like the Consolas font for coding. Being a Microsoft (!) font, it is not installed by default. Since we're going to be looking at a lot of terminal output and code, let's install it now.

There are two ways we can install it. If you bought **Microsoft Office for Mac**, install that and Consolas will be installed as well.

If you don't have Office, follow these steps:

    $ brew install cabextract
    $ cd ~/Downloads
    $ mkdir consolas
    $ cd consolas
    $ curl -O http://download.microsoft.com/download/f/5/a/f5a3df76-d856-4a61-a6bd-722f52a5be26/PowerPointViewer.exe
    $ cabextract PowerPointViewer.exe
    $ cabextract ppviewer.cab
    $ open CONSOLA*.TTF

And click **Install Font**. Thanks to Alexander Zhuravlev for his [post](http://blog.ikato.com/post/15675823000/how-to-install-consolas-font-on-mac-os-x).

## Beautiful terminal

Since we spend so much time in the terminal, we should try to make it a more pleasant and colorful place. What follows might seem like a lot of work, but trust me, it'll make the development experience so much better.

Let's go ahead and start by changing the font. In **iTerm > Preferences...**, under the tab **Profiles**, section **Text**, change both fonts to **Consolas 13pt**.

Now let's add some color. I'm a big fan of the [Solarized](http://ethanschoonover.com/solarized) color scheme. It is supposed to be scientifically optimal for the eyes. I just find it pretty.

Scroll down the page and download the latest version. Unzip the archive. In it you will find the `iterm2-colors-solarized` folder with a `README.md` file, but I will just walk you through it here:

- In **iTerm2 Preferences**, under **Profiles** and **Colors**, go to **Load Presets... > Import...**, find and open the two **.itermcolors** files we downloaded.
- Go back to **Load Presets...** and select **Solarized Dark** to activate it. Voila!

**Note**: You don't have to do this, but there is one color in the **Solarized Dark** preset I don't agree with, which is *Bright Black*. You'll notice it's too close to *Black*. So I change it to be the same as *Bright Yellow*, i.e. **R 83 G 104 B 112**.

Not a lot of colors yet. We need to tweak a little bit our Unix user's profile for that. This is done (on OS X and Linux), in the `~/.bash_profile` text file (`~` stands for the user's home directory).

We'll come back to the details of that later, but for now, just download the files [.bash_profile](/nicolahery/mac-dev-setup/blob/master/.bash_profile), [.bash_prompt](/nicolahery/mac-dev-setup/blob/master/.bash_prompt), [.aliases](/nicolahery/mac-dev-setup/blob/master/.aliases) attached to this document into your home directory (`.bash_profile` is the one that gets loaded, I've set it up to call the others):

    $ cd ~
    $ curl -O https://raw.github.com/nicolahery/mac-dev-setup/master/.bash_profile
    $ curl -O https://raw.github.com/nicolahery/mac-dev-setup/master/.bash_prompt
    $ curl -O https://raw.github.com/nicolahery/mac-dev-setup/master/.aliases
    
With that, open a new terminal tab (Cmd+T) and see the change! Try the list commands: `ls`, `ls -lh` (aliased to `ll`), `ls -lha` (aliased to `la`).

At this point you can also change your computer's name, which shows up in this terminal prompt. If you want to do so, go to **System Preferences** > **Sharing**. For example, I changed mine from "Nicolas's MacBook Air" to just "MacBook Air", so it shows up as `MacBook-Air` in the terminal.

Now we have a terminal we can work with!

(Thanks to Mathias Bynens for his awesome [dotfiles](https://github.com/mathiasbynens/dotfiles).)

## Git

What's a developer without [Git](http://git-scm.com/)? To install, simply run:

    $ brew install git
    
When done, to test that it installed fine you can run:

    $ git --version
    
And `$ which git` should output `/usr/local/bin/git`.

Let's set up some basic configuration. Download the [.gitconfig](/nicolahery/mac-dev-setup/blob/master/.gitconfig) file to your home directory:

    $ cd ~
    $ curl -O https://raw.github.com/nicolahery/mac-dev-setup/master/.gitconfig

It will add some color to the `status`, `branch`, and `diff` Git commands, as well as a couple aliases. Feel free to take a look at the contents of the file, and add to it to your liking.

**Optional if you don't want to login all the time** 
Next, we'll define your Git user (should be the same name and email you use for [GitHub](https://github.com/) and [Heroku](http://www.heroku.com/)):

    $ git config --global user.name "Your Name Here"
    $ git config --global user.email "your_email@youremail.com"

They will get added to your `.gitconfig` file.

To push code to your GitHub repositories, we're going to use the recommended HTTPS method (versus SSH). So you don't have to type your username and password everytime, let's enable Git password caching as described [here](https://help.github.com/articles/set-up-git):

    $ git config --global credential.helper osxkeychain
    
**Note**: On a Mac, it is important to remember to add `.DS_Store` (a hidden OS X system file that's put in folders) to your `.gitignore` files. You can take a look at this repository's [.gitignore](/nicolahery/mac-dev-setup/blob/master/.gitignore) file for inspiration.
    
## Sublime Text

With the terminal, the text editor is a developer's most important tool. Everyone has their preferences, but unless you're a hardcore [Vim](http://en.wikipedia.org/wiki/Vim_(text_editor)) user, a lot of people are going to tell you that [Sublime Text](http://www.sublimetext.com/) is currently the best one out there.

Go ahead and [download](http://www.sublimetext.com/) it. Open the **.dmg** file, drag-and-drop in the **Applications** folder, you know the drill now. Launch the application.

**Note**: At this point I'm going to create a shorcut on the OS X Dock for both for Sublime Text and iTerm. To do so, right-click on the running application and select **Options > Keep in Dock**.

Sublime Text is not free, but I think it has an unlimited "evaluation period". Anyhow, we're going to be using it so much that even the seemingly expensive $60 price tag is worth every penny. If you can afford it, I suggest you [support](http://www.sublimetext.com/buy) this awesome tool. :)

Just like the terminal, let's configure our editor a little. Go to **Sublime Text 2 > Preferences > Settings - User** and paste the following in the file that just opened:

```json
{
    "font_face": "Consolas",
    "font_size": 13,
    "rulers":
    [
        79
    ],
    "highlight_line": true,
    "bold_folder_labels": true,
    "highlight_modified_tabs": true,
    "tab_size": 2,
    "translate_tabs_to_spaces": true,
    "word_wrap": false,
    "indent_to_bracket": true
}
```
    
Feel free to tweak these to your preference. When done, save the file and close it.

I use tab size 2 for everything except Python and Markdown files, where I use tab size 4. If you have a Python and Markdown file handy (or create dummy ones with `$ touch dummy.py`), for each one, open it and go to **Sublime Text 2 > Preferences > Settings - More > Syntax Specific - User** to paste in:

```json
{
    "tab_size": 4
}
```

Now for the color. I'm going to change two things: the **Theme** (which is how the tabs, the file explorer on the left, etc. look) and the **Color Scheme** (the colors of the code). Again, feel free to pick different ones, or stick with the default.

A popular Theme is the [Soda Theme](https://github.com/buymeasoda/soda-theme). To install it, run:

    $ cd ~/Library/Application\ Support/Sublime\ Text\ 2/Packages/
    $ git clone https://github.com/buymeasoda/soda-theme/ "Theme - Soda"
    
Then go to **Sublime Text 2 > Preferences > Settings - User** and add the following two lines:

    "theme": "Soda Dark.sublime-theme",
    "soda_classic_tabs": true

Restart Sublime Text for all changes to take affect (Note: on the Mac, closing all windows doesn't close the application, you need to hit **Cmd+Q**).

The Soda Theme page also offers some [extra color schemes](https://github.com/buymeasoda/soda-theme#syntax-highlighting-colour-schemes) you can download and try. But to be consistent with my terminal, I like to use the **Solarized** Color Scheme, which already ships with Sublime Text. To use it, just go to **Sublime Text 2 > Preferences > Color Scheme > Solarized (Dark)**. Again, this is really according to personal flavors, so pick what you want.

Sublime Text 2 already supports syntax highlighting for a lot of languages. I'm going to install a couple that are missing:

    $ cd ~/Library/Application\ Support/Sublime\ Text\ 2/Packages/
    $ git clone https://github.com/jashkenas/coffee-script-tmbundle CoffeeScript
    $ git clone https://github.com/miksago/jade-tmbundle Jade
    $ git clone https://github.com/danro/LESS-sublime.git LESS
    $ git clone -b SublimeText2 https://github.com/kuroir/SCSS.tmbundle.git SCSS
    $ git clone https://github.com/nrw/sublime-text-handlebars Handlebars

Let's create a shortcut so we can launch Sublime Text from the command-line:

    $ cd ~
    $ mkdir bin
    $ ln -s "/Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl" ~/bin/subl

Now I can open a file with `$ subl myfile.py` or start a new project in the current directory with `$ subl .`. Pretty cool.

Sublime Text is very extensible. For now we'll leave it like that, we already have a solid installation. To add more in the future, a good place to start would be to install the [Sublime Package Control](http://wbond.net/sublime_packages/package_control/installation).

## Vim (Optional)

Although Sublime Text will be our main editor, it is a good idea to learn some very basic usage of [Vim](http://www.vim.org/). It is a very popular text editor inside the terminal, and is usually pre-installed on any Unix system.

For example, when you run a Git commit, it will open Vim to allow you to type the commit message.

I suggest you read a tutorial on Vim. Grasping the concept of the two "modes" of the editor, **Insert** (by pressing `i`) and **Normal** (by pressing `Esc` to exit Insert mode), will be the part that feels most unatural. After that it's just remembering a few important keys.

Vim's default settings aren't great, and you could spend a lot of time tweaking your configuration (the `.vimrc` file). But if you're like me and just use Vim occasionally, you'll be happy to know that [Tim Pope](https://github.com/tpope) has put together some sensible defaults to quickly get started.

First, install [pathogen.vim](https://github.com/tpope/vim-pathogen) by running:

    $ mkdir -p ~/.vim/autoload ~/.vim/bundle
    $ curl -Sso ~/.vim/autoload/pathogen.vim \
        https://raw.github.com/tpope/vim-pathogen/master/autoload/pathogen.vim

Then create a file `~/.vimrc` (you can use `$ subl ~/.vimrc`), and paste in the following:

    execute pathogen#infect()
    syntax on
    filetype plugin indent on

And finally, install the Vim "sensible defaults" by running:

    $ cd ~/.vim/bundle
    $ git clone git://github.com/tpope/vim-sensible.git

With that, Vim will look a lot better next time you open it!

## Python

OS X, like Linux, ships with [Python](http://python.org/) already installed. But you don't want to mess with the system Python (some system tools rely on it, etc.), so we'll install our own version with Homebrew. It will also allow us to get the very latest version of Python 2.7.

The following command will install Python 2.7 and any dependencies required (it can take a few minutes to build everything):

    $ brew install python
    
When finished, you should get a summary in the terminal. Running `$ which python` should output `/usr/local/bin/python`.

It also installed [Pip]() (and its dependency [Distribute]()), which is the package manager for Python. Let's upgrade them both:

    $ pip install --upgrade distribute
    $ pip install --upgrade pip
    
Executable scripts from Python packages you install will be put in `/usr/local/share/python`, so let's add it to the `$PATH`. To do so, we'll create a `.path` text file in the home directory (I've already set up `.bash_profile` to call this file):

    $ cd ~
    $ subl .path
    
And add these lines to `.path`:

```bash
PATH=/usr/local/share/python:$PATH
export PATH
```
    
Save the file and open a new terminal to take the new `$PATH` into account (everytime you open a terminal, `.bash_profile` gets loaded).

### Pip Usage

Here are a couple Pip commands to get you started. To install a Python package:

    $ pip install <package>

To upgrade a package:

    $ pip install --upgrade <package>
        
To see what's installed:

    $ pip freeze
    
To uninstall a package:

    $ pip uninstall <package>

## Virtualenv

[Virtualenv](http://www.virtualenv.org/) is a tool that creates an isolated Python environment for each of your projects. For a particular project, instead of installing required packages globally, it is best to install them in an isolated folder in the project (say a folder named `venv`), that will be managed by virtualenv.

The advantage is that different projects might require different versions of packages, and it would be hard to manage that if you install packages globally. It also allows you to keep your global `/usr/local/lib/python2.7/site-packages` folder clean, containing only critical or big packages that you always need (like IPython, Numpy).

### Install

To install virtualenv, simply run:

    $ pip install virtualenv

### Usage

Let's say you have a project in a directory called `myproject`. To set up virtualenv for that project:

    $ cd myproject/
    $ virtualenv venv --distribute
    
If you want your virtualenv to also inherit globally installed packages (like IPython or Numpy mentioned above), use:

    $ virtualenv venv --distribute --system-site-packages

These commands create a `venv` subdirectory in your project where everything is installed. You need to **activate** it first though (in every terminal where you are working on your project):

    $ source venv/bin/activate
    
You should see a `(venv)` appear at the beginning of your terminal prompt indicating that you are working inside the virtualenv. Now when you install something:

    $ pip install <package>

It will get installed in the `venv` folder, and not conflict with other projects.

**Important**: Remember to add `venv` to your project's `.gitignore` file so you don't include all of that in your source code!

As mentioned earlier, I like to install big packages (like Numpy), or packages I always use (like IPython) globally. All the rest I install in a virtualenv.

## IPython (Optional for heavy visualization work)

[IPython](http://ipython.org/) is an awesome project which provides a much better Python shell than the one you get from running `$ python` in the command-line. It has many cool functions (running Unix commands from the Python shell, easy copy & paste, creating Matplotlib charts in-line, etc.) and I'll let you refer to the [documentation](http://ipython.org/ipython-doc/stable/index.html) to discover them.

### Install

Before we install IPython, we'll need to get some dependencies. Run the following:

    $ brew update # Always good to do
    $ brew install zeromq # Necessary for pyzmq
    $ brew install pyqt # Necessary for the qtconsole
    
It may take a few minutes to build these.

Once it's done, we can install IPython with all the available options:

    $ pip install ipython[zmq,qtconsole,notebook,test]

### Usage

You can launch IPython from the command line with `$ ipython`, but what's more interesting is to use its [QT Console](http://ipython.org/ipython-doc/stable/interactive/qtconsole.html). Launch the QT Console by running:

    $ ipython qtconsole
    
You can also customize the font it uses:

    $ ipython qtconsole --ConsoleWidget.font_family="Consolas" --ConsoleWidget.font_size=13
    
And since I'm lazy and I don't want to type or copy & paste that all the time, I'm going to create an alias for it. Create a `.extra` text file in your home directory with `$ subl ~/.extra` (I've set up `.bash_profile` to load `.extra`), and add the following line:

```bash
alias ipy='ipython qtconsole --ConsoleWidget.font_family="Consolas" --ConsoleWidget.font_size=13'
```
    
Open a fresh terminal. Now when you run `$ ipy`, it will launch the QT Console with your configured options.

To use the in-line Matplotlib functionality (nice for scientific computing), run `$ ipy --pylab=inline`.

## Numpy and Scipy

The [Numpy](http://numpy.scipy.org/) and [Scipy](http://www.scipy.org/SciPy) scientific libraries for Python are always a little tricky to install from source because they have all these dependencies they need to build correctly. Luckily for us, [Samuel John](http://www.samueljohn.de/) has put together some [Homebrew formulae](https://github.com/samueljohn/homebrew-python) to make it easier to install these Python libraries.

First, grab the special formulae (which are not part of Homebrew core):

    $ brew tap samueljohn/python
    $ brew tap homebrew/science
    
Then, install the `gfortran` dependency which we will need to build the libraries:

    $ brew install gfortran
    
You probably also need the `nose` python dependency:

    $ pip install nose
    
Finally, you can install Numpy and Scipy with:

    $ brew install numpy
    $ brew install scipy
    
(It may take a few minutes to build.)

## MySQL

### Install

We will install [MySQL](http://www.mysql.com/) using Homebrew, which will also install some header files needed for MySQL bindings in different programming languages (MySQL-Python for one).

To install, run:

    $ brew update # Always good to do
    $ brew install mysql

As you can see in the ouput from Homebrew, before we can use MySQL we first need to set it up with:

    $ unset TMPDIR
    $ mkdir /usr/local/var
    $ mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp

### Usage

To start the MySQL server, use the `mysql.server` tool:

    $ mysql.server start
    
To stop it when you are done, run:

    $ mysql.server stop
    
You can see the different commands available for `mysql.server` with:

    $ mysql.server --help
    
To connect with the command-line client, run:

    $ mysql -uroot
    
(Use `exit` to quit the MySQL shell.)

**Note**: By default, the MySQL user `root` has no password. It doesn't really matter for a local development database. If you wish to change it though, you can use `$ mysqladmin -u root password 'new-password'`.

### MySQL Workbench (**Optional or use SequelPro**)

In terms of a GUI client for MySQL, I'm used to the official and free [MySQL Workbench](http://www.mysql.com/products/workbench/). But feel free to use whichever you prefer.

You can find the MySQL Workbench download [here](http://www.mysql.com/downloads/workbench/). (**Note**: It will ask you to sign in, you don't need to, just click on "No thanks, just start my download!" at the bottom.)

## Node.js

Install [Node.js](http://nodejs.org/) with Homebrew:

    $ brew update
    $ brew install node
    
The formula also installs the [npm](https://npmjs.org/) package manager. However, as suggested by the Homebrew output, we need to add `/usr/local/share/npm/bin` to our path so that npm-installed modules with executables will have them picked up.

To do so, add this line to your `~/.path` file, before the `export PATH` line:

```bash
PATH=/usr/local/share/npm/bin:$PATH
```
        
Open a new terminal for the `$PATH` changes to take effect.

**Careful/Deprecated**: Other tools such as Cordova look for this inside of XCode.app package.  I skipped this step
We also need to tell npm where to find the Xcode Command Line Tools, by running:

    $ sudo xcode-select -switch /usr/bin

Node modules are installed locally in the `node_modules` folder of each project by default, but there are at least two that are worth installing globally. Those are [CoffeeScript](http://coffeescript.org/) and [Grunt](http://gruntjs.com/):

    $ npm install -g coffee-script
    $ npm install -g grunt-cli

### Npm usage

To install a package:

    $ npm install <package> # Install locally
    $ npm install -g <package> # Install globally

To install a package and save it in your project's `package.json` file:

    $ npm install <package> --save

To see what's installed:

    $ npm list # Local
    $ npm list -g # Global

To find outdated packages (locally or globally):

    $ npm outdated [-g]

To upgrade all or a particular package:

    $ npm update [<package>]

To uninstall a package:

    $ npm uninstall <package>

## Ruby and RVM

Like Python, [Ruby](http://www.ruby-lang.org/) is already installed on Unix systems. But we don't want to mess around with that installation. More importantly, we want to be able to use the latest version of Ruby.

### Install

When installing Ruby, best practice is to use [RVM](https://rvm.io/) (Ruby Version Manager) which allows you to manage multiple versions of Ruby on the same machine. Installing RVM, as well as the latest version of Ruby, is very easy. Just run:

    $ curl -L https://get.rvm.io | bash -s stable --ruby
    
When it is done, both RVM and a fresh version of Ruby 2.0 are installed. The following line was also automatically added to your `.bash_profile`:

```bash
[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*
```

I prefer to move that line to the `.extra` file, keeping my `.bash_profile` clean. I suggest you do the same.

After that, start a new terminal and run:

    $ type rvm | head -1
    
You should get the output `rvm is a function`.

### Usage

The following command will show you which versions of Ruby you have installed:

    $ rvm list

The one that was just installed, Ruby 2.0, should be set as default. When managing multiple versions, you switch between them with:

    $ rvm use system # Switch back to system install (1.8)
    $ rvm use 2.0.0 --default # Switch to 2.0.0 and sets it as default

Run the following to make sure the version you want is being used (in our case, the just-installed Ruby 1.9.3):

    $ which ruby
    $ ruby --version

You can install another version with:

    $ rvm install 1.9.3

To update RVM itself, use:

    $ rvm get stable
    
[RubyGems](http://rubygems.org/), the Ruby package manager, was also installed:

    $ which gem
    
Update to its latest version with:

    $ gem update --system
    
To install a "gem" (Ruby package), run:

    $ gem install <gemname>
        
To install without generating the documentation for each gem (faster):

    $ gem install <gemname> --no-document
        
To see what gems you have installed:

    $ gem list
    
To check if any installed gems are outdated:

    $ gem outdated
    
To update all gems or a particular gem:

    $ gem update [<gemname>]
    
RubyGems keeps old versions of gems, so feel free to do come cleaning after updating:

    $ gem cleanup
    
I mainly use Ruby for the CSS pre-processor [Compass](http://compass-style.org/), which is built on top of [Sass](http://sass-lang.com/):

    $ gem install compass --no-document

## Heroku

[Heroku](http://www.heroku.com/), if you're not already familiar with it, is a [Platform-as-a-Service](http://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) that makes it really easy to deploy your apps online. There are other similar solutions out there, but Heroku was among the first and is currently the most popular. Not only does it make a developer's life easier, but I find that having Heroku deployment in mind when building an app forces you to follow modern app development [best practices](http://www.12factor.net/).

### Install

**NOTE**: If you previously installed using the Heroku Toolbelt install from Heroku's website its a good idea to remove it.  Since there are no
real instructions you will need to rm -rf /usr/local/heroku /usr/bin/heroku /usr/local/foreman /usr/bin/foreman

Assuming that you have an account (sign up if you don't), let's install the [Heroku Client](https://devcenter.heroku.com/articles/using-the-cli) for the command-line. Heroku offers a Mac OS X installer, the [Heroku Toolbelt](https://toolbelt.heroku.com/), that includes the client. But for these kind of tools, I prefer using Homebrew. It allows us to keep better track of what we have installed. Luckily for us, Homebrew includes a `heroku-toolbelt` formula:
**NOTE**: The homebrew formula does not include foreman for some reason as of 7/1/2013

    $ brew install heroku-toolbelt
    
The formula might not have the latest version of the Heroku Client, which is updated pretty often. Let's update it now:

    $ heroku update
    
Don't be afraid to run `heroku update` every now and then to always have the most recent version.

### Usage

Login to your Heroku account using your email and password:

    $ heroku login
    
If this is a new account, and since you don't already have a public **SSH key** in your `~/.ssh` directory, it will offer to create one for you. Say yes! It will also upload the key to your Heroku account, which will allow you to deploy apps from this computer.

If it didn't offer create the SSH key for you (i.e. your Heroku account already has SSH keys associated with it), you can do so manually by running:

     $ mkdir ~/.ssh
     $ ssh-keygen -t rsa
     
Keep the default file name and skip the passphrase by just hitting Enter both times. Then, add the key to your Heroku account:

    $ heroku keys:add
    
Once the key business is done, you're ready to deploy apps! Heroku has a great [Getting Started](https://devcenter.heroku.com/articles/python) guide, so I'll let you refer to that (the one linked here is for Python, but there is one for every popular language). Heroku uses Git to push code for deployment, so make sure your app is under Git version control. A quick cheat sheet (if you've used Heroku before):

    $ cd myapp/
    $ heroku create myapp
    $ git push heroku master
    $ heroku ps
    $ heroku logs -t
    
The [Heroku Dev Center](https://devcenter.heroku.com/) is full of great resources, so be sure to check it out!

## MongoDB

[MongoDB](http://www.mongodb.org/) is a popular [NoSQL](http://en.wikipedia.org/wiki/NoSQL) database.

### Install

Installing it is very easy through Homebrew:

    $ brew update
    $ brew install mongo

### Usage

In a terminal, start the MongoDB server:

    $ mongod

In another terminal, connect to the database with the Mongo shell using:

    $ mongo

I'll let you refer to MongoDB's [Getting Started](http://docs.mongodb.org/manual/tutorial/getting-started/) guide for more!

## Redis

[Redis](http://redis.io/) is a blazing fast, in-memory, key-value store, that uses the disk for persistence. It's kind of like a NoSQL database, but there are a lot of [cool things](http://oldblog.antirez.com/post/take-advantage-of-redis-adding-it-to-your-stack.html) that you can do with it that would be hard or inefficient with other database solutions. For example, it's often used as session management or caching by web apps, but it has many other uses.

### Install

To install Redis, use Homebrew:

    $ brew update
    $ brew install redis

### Usage

Start a local Redis server using the default configuration settings with:

    $ redis-server

For advanced usage, you can tweak the configuration file at `/usr/local/etc/redis.conf` (I suggest making a backup first), and use those settings with:

    $ redis-server /usr/local/etc/redis.conf

In another terminal, connect to the server with the Redis command-line interface using:

    $ redis-cli

I'll let you refer to Redis' [documentation](http://redis.io/documentation) or other tutorials for more information.

## VirtualBox

VirtualBox provides a virtualization environment allowing you to run other operating system images such as Linux.  This is particularly useful when doing cloud development of backend systems to ensure compatability.

Download and install the latest VirtualBox from here [virtualbox site](https://www.virtualbox.org)

## Vagrant

Vagrant is a configuration management system for virtualized environments such as those running VirtualBox, EC2, VMWare.  Vagrant relies on Ruby scripts and some symlink magic to allow management of the virtual machine from within
the host operating system (i.e. your Mac OS environment).  Files under the /vagrant directory will show up inside of the virtual machine thereby creating a bridge of sorts.  Vagrant makes it easier to do configuration management
that is portable across developer machines.  Since most virtualized environments like EC2 use linux this provides a way to create a linux image and run it on your Mac, thereby, reducing surprises when you deploy to EC2.  Vagrant
has plugins for EC2 to allow easier deployment as well.  Chef and Puppet would be complimentary tools to Vagrant.

Vagrant can be found here along with [download](http://downloads.vagrantup.com) and [install instructions](http://docs.vagrantup.com/v2/installation/index.html)

Step 1: Install the Vagrant AWS plugin for future use

    $ vagrant plugin install vagrant-aws    

Step 2: Next you must identify a Vagrant box image corresponding to VirtualBox (or other provider).  The base linux images that you use are up to you, however, Ubuntu is a popular option and one of the latest versions can be found 
here [Ubuntu vagrant boxes](http://cloud-images.ubuntu.com/vagrant/) and [Ubuntu vagrant sources](https://launchpad.net/ubuntu/raring/i386/vagrant).  Also there are [official third party Vagrant boxes](http://www.vagrantbox.es) 
and [Puppet Labs boxes](http://puppet-vagrant-boxes.puppetlabs.com)

Once you have selected the box image of your choice, you can either download it ahead of time/use an exisitng box or use the alternative method below to have Vagrant automatically download the box.

**NOTE**: There is a bug with Virtual Box 2.14 (reportedly 2.10 does not have this issue).  Virtual Box 2.14 apparently requires a [manifest](https://github.com/mitchellh/vagrant/issues/1847) in addition to the ovf file.  This can be fixed one of two ways.
Either uninstall your Virtual Box 2.14 and get the older 2.10 version or for each Vagrant box create a manifest such that VirtualBox can open the box

    $ vagrant box add {title} {url or filepath} //This is the box image you previously decided to use
    $ cd ~/.vagrant.d/boxes/<BaseBoxName>/virtualbox
    $ openssl sha1 *.vmdk *.ovf > box.mf  //This will generate the manifest that VirtualBox's bug requires


Step 3: Now create the VagrantFile for your project

    $ cd ~/Development
    $ mkdir <your project dir>
    $ cd <your project dir>
    $ vagrant init {title} //This creates a VagrantFile in the current directory with the box name
    $ vagrant up //This startups up the box using the VagrantFile as configuration

Alternatively you can just manually edit a blank or previously created VagrantFile to point it to the right box image and Vagrant will download and assign it to the indicated name when you do vagrant up.

    $ mkdir <your project dir>
    $ cd <your project dir>
    $ vagrant init {title} //This creates a VagrantFile in the current directory with the box name i.e. config.vm.box={title}
    $ subl VagrantFile //Set config.vm.box_url = <your box file url or file path>
    $ vagrant up //This startups up the box using the VagrantFile and will download the box image and assign to {title}

If you would like to use Vagrant with an alternate provider such as AWS a few modification will be needed:

    $ ... make sure you have a Vagrantfile for AWS box before you do a vagrant up and want to use aws see below
    $ vagrant box add ...
    $ vagrant init ...
    $ vagrant up --provider=aws //This ensures that the AWS provider is used with vagrant and prior to this the AWS tools must be installed.

Additional resources:

- AWS detailed [instructions and code](https://github.com/mitchellh/vagrant-aws) for converting and AMI to Vagrant with AWS provider
- Here is how to use Puppet and Vagrant [together](http://www.andrewmunsell.com/blog/development-environments-with-vagrant-and-puppet/)
- Dummy box example [instructions](http://serverascode.com/2013/04/25/where-to-find-vagrant-boxes.html) for creating a blank VagrantFile based on Ubuntu raw images
- Lamp box example [instructions](https://github.com/marcojanssen/vagrant-puppet-lamp) with Vagrant (probably want to modify VagrantFile to use i386 image).

## Ubuntu

After picking a VirtualBox compatible Ubuntu image it is likely that this image will need to be updated.  Ubuntu's package manager is apt-get.  Login to the Ubuntu server (i.e. vagrant ssh) and perform an update:

    $ vagrant up //Start up your Ubuntu box using vagrant and then login to it and update it
    $ vagrant ssh //login to the ubuntu box
    $ sudo apt-get update //update the sources
    $ sudo apt-get upgrade //make sure everything is up to date

For reference here is the [apt command guide](https://help.ubuntu.com/community/AptGet/Howto)
You may need or want to install the VirtualBox guest tools into Ubuntu following [this](http://virtualboxes.org/doc/installing-guest-additions-on-ubuntu/).  This may require setting up kernel modules so follow [this](http://www.howtoforge.com/building-kernel-modules-with-module-assistant-on-debian-lenny)

## Docker

Docker provides a type of subvirtualization using LXC (linux containers).  This is less overhead and useful in a multi-tentant cloud hosted environment where different users need their software isolated while at the same time
the provider needs to contain costs by reusing a shared pool of virtual machines.  Docker/LXC based isolation is essentially built into the latest linux kernels and creates process control groups which the OS itself makes sure
are isolated.

**NOTE**: The below instructions should be modified in that the latest Docker repository only has a Vagrantfile for Ubuntu 12.X.  If you want to use Ubuntu 13.x (recommended) which has the 3.8 kernel already in place then
an alternate Vagrantfile or modifications to this existing Vagrantfile will be needed.  Info can be pulled from [here](http://docs.docker.io/en/latest/installation/ubuntulinux/#ubuntu-raring)

Instructions for installing Docker inside of a Vagrant/VirtualBox VM are [here](http://docs.docker.io/en/latest/installation/vagrant/).

## Amazon Tools

Step 0: Here are instructions using [homebrew install of amazon tools](http://clayrichardson.me/2013/03/29/brew-install-all-available-aws-tools/)

Once you have followed the homebrew instructions, you will notice that there are several instructions for each tool involving setup of environment variables, access keys, and certificates.  Captured below is a summary of all the environment variables and access key and certificate generation.
This assumes that you followed the homebrew install method for amazon tools.  Create a .ec2 directory to hold certificates.

Step 1: Download your Amazon access key.  If you have an IAM account you'll need to login via your IAM url and account and 

    //This is to setup your access key and access id for AWS.  Several amazon tools will use an env variable pointing to this file to authenticate
    $ cd ~
    $ vi .aws-credentials-master
    //Now add your access id and access key to this credentials file and save it
    AWSAccessKeyId=<Your access id from AWS root account or IAM account>
    AWSSecretKey=<Your access id from IAM user account (or AWS root account)>
    $ chmod 600 .aws-credentials-master
    
Step 2: If using IAM you'll need to create your own self signed X509 certificate and then upload to AWS.  Here is some good [background](https://help.ubuntu.com/community/EC2StartersGuide) reading on what we will be setting up.

NOTE: If you are not using an IAM user account and instead using the root account then skip the steps below and instead follow steps 1-6 *only* in this [alternate method](http://www.robertsosinski.com/2008/01/26/starting-amazon-ec2-with-mac-os-x/)
        
First, for IAM user accounts if you do not have an X509 certificate you will need to create one locally and upload it to your IAM user account
    
    //This is to setup your amazon certificate and private key files (i.e. pk-*.pem and cert-*.pem)
    $ cd ~
    $ mkdir .ec2
    $ chmod 700 .ec2
    $ cd .ec2
    
Second, follow [these instructions](http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_UploadCertificate.html) to create your certificate and private key with the following modifications:
    
- Under the sections in this document labelled "Create a Private Key" name your files in step 1 and 2: pk-private-key.pem and pk-private-key-in-PCKS8-format.pem
- Under "Create the User Signing Certificate" make sure you name the certificate file cert-<your certname>.pem
    
Third, make sure you pk-*.pem and cert-*.pem files are in your .ec2 directory
    
    $ cd ~/.ec2
    $ chmod 600 *.pem
    
Step 3: Setup your .bash_profile (or if you use zsh setup in your zsh profile)

    $ cd ~
    $ vi .bash_profile
    //Now add the following (or ensure you have it already) to your .bash_profile
    export JAVA_HOME="$(/usr/libexec/java_home)"
    export EC2_PRIVATE_KEY="$(/bin/ls "$HOME"/.ec2/pk-*.pem | /usr/bin/head -1)"
    export EC2_CERT="$(/bin/ls "$HOME"/.ec2/cert-*.pem | /usr/bin/head -1)"
    export AWS_AUTO_SCALING_HOME="/usr/local/Library/LinkedKegs/auto-scaling/jars"
    export EC2_HOME="/usr/local/Library/LinkedKegs/ec2-api-tools/jars"
    export AWS_CLOUDFORMATION_HOME="/usr/local/Library/LinkedKegs/aws-cfn-tools/jars"
    export AWS_ELASTICACHE_HOME="/usr/local/Cellar/aws-elasticache/1.7.000/libexec"
    export AWS_CLOUDWATCH_HOME="/usr/local/Library/LinkedKegs/cloud-watch/jars"
    export SERVICE_HOME="$AWS_CLOUDWATCH_HOME"
    export AWS_IAM_HOME="/usr/local/opt/aws-iam-tools/jars"
    export AWS_SNS_HOME="/usr/local/Library/LinkedKegs/aws-sns-cli/jars"
    export AWS_CREDENTIAL_FILE=$HOME/.aws-credentials-master
    export EC2_AMITOOL_HOME="/usr/local/Library/LinkedKegs/ec2-ami-tools/jars"
    export AWS_ELB_HOME="/usr/local/Library/LinkedKegs/elb-tools/jars"
    export AWS_RDS_HOME="/usr/local/Cellar/rds-command-line-tools/1.12.002/libexec"

Step 4: Verify that everything is working.  A couple of commands should tell you if connected:
    
    $ ec2-describe-regions //Get a list of available regions to host
    $ ec2-describe-images -o amazon  //Get a list of available images
    
Step 5: Select your AWS images to use.  If you are using vagrant to manage then see the vagrant section.

- Amazon directory of [AMI Images](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonLinuxAMIBasics.html)
- Ubuntu can be found here [Ubuntu official images](http://cloud-images.ubuntu.com)

Additional resources:

- [AWS Setup for PHP](http://phpmaster.com/from-zero-to-cloud-2/)
- [Newer manual install](http://www.admon.org/setup-amazon-ec2-api-tools-on-macbook/) 

## Projects folder

This really depends on how you want to organize your files, but I like to put all my version-controlled projects in `~/Development`. Other documents I may have, or things not yet under version control, I like to put in `~/Dropbox` (if you have Dropbox installed), or `~/Documents`.

## Apps

Here is a quick list of some apps I use, and that you might find useful as well:

- [Dropbox](https://www.dropbox.com/): File syncing to the cloud. I put all my documents in Dropbox. It syncs them to all my devices (laptop, mobile, tablet), and serves as a backup as well! **(Free for 2GB)**
- [Google Drive](https://drive.google.com/): File syncing to the cloud too! I use Google Docs a lot to collaborate with others (edit a document with multiple people in real-time!), and sometimes upload other non-Google documents (pictures, etc.), so the app comes in handy for that. **(Free for 5GB)**
- [1Password](https://agilebits.com/onepassword): Allows you to securely store your login and passwords. Even if you only use a few different passwords (they say you shouldn't!), this is really handy to keep track of all the accounts you sign up for! Also, they have a mobile app so you always have all your passwords with you (syncs with Dropbox). A little pricey though. There are free alternatives. **($50 for Mac app, $18 for iOS app)**
- [Marked](http://markedapp.com/): As a developer, most of the stuff you write ends up being in [Markdown](http://daringfireball.net/projects/markdown/). In fact, this `README.md` file (possibly the most important file of a GitHub repo) is indeed in Markdown, written in Sublime Text, and I use Marked to preview the results everytime I save. **($4)**
- [Path Finder](http://cocoatech.com/pathfinder/): I love OSX, it's Unix so great for developers, and all of it just works and looks pretty! Only thing I "miss" from Windows (OMG what did he say?), is a decent file explorer. I think Finder is a pain to use. So I gladly paid for this alternative, but I understand others might find it expensive just to not have to use Finder. **($40)**
- [Evernote](https://evernote.com/): If I don't write something down, I'll forget it. As a developer, you learn so many new things every day, and technology keeps changing, it would be insane to want to keep it all in your head. So take notes, sync them to the cloud, and have them on all your devices. To be honest, I switched to [Simplenote](http://simplenote.com/) because I only take text notes, and I got tired of Evernote putting extra spaces between paragraphs when I copy & pasted into other applications. Simplenote is so much better for text notes (and it supports Markdown!). **(Both are free)**
- [Moom](http://manytricks.com/moom/): Don't waste time resizing and moving your windows. Moom makes this very easy. **($10)**


