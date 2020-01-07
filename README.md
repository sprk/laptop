Laptop
======

Laptop is a script to set up a macOS laptop for web and mobile development.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages
based on what is already installed on the machine.

Requirements
------------

We support:

* macOS High Sierra (10.13)
* macOS Mojave (10.14)
* macOS Catalina (10.15)

Older versions may work but aren't regularly tested.
Bug reports for older versions are welcome.

Install
-------

Open Terminal and Install xcode select:

`xcode-select --install`

Create you SSH key and link to your Github Account:

`ssh-keygen -C your.name@spark.re`

Enter a location to save the save the key. Press enter
to save it in the default location, or enter a new location.
If you save it in the default location, all ssh connections
will use this key by default. You should only need to save
it in a non default location if you use multiple keys.

`# Enter file in which to save the key (/Users/USERNAME/.ssh/id_rsa):`

Can also keep it simple and just press enter when prompted for the above.

Be sure to add a password to your key to ensure the
security of our servers. This won't need to be a password
you remember, as the ssh agent can remember it for you
(see below). So that being said, the best course of action
is to use a generated password through Dashlane.

Set your identity in the SSH Config:
This will fix an issue when running deployments where
you get permission denied from Github, since our local private
keys are used by Capistranto when pulling the code.

```
touch ~/.ssh/config
echo 'UseKeychain yes' >> ~/.ssh/config
echo 'AddKeysToAgent yes' >> ~/.ssh/config
echo 'IdentityFile ~/.ssh/id_rsa' >> ~/.ssh/config
```

Add the new SSH key to your terminal instance to prevent need to enter SSH password:

`ssh-add -K`

Link your SSH key with your Github Account. First, copy the
SSH key to your clipboard.

`cat ~/.ssh/id_rsa.pub | pbcopy`

Github > Profile > Settings > SSH and GPG Keys > New SSH key.

Download the computer setup repository to your root directory and move the mac script there too:

```
cd ~ && git clone git@github.com:sprk/laptop.git
cp ~/laptop/mac ~/mac
```

Review the script (avoid running scripts you haven't read!):
```
less mac
```

Execute the downloaded script:
```
sh mac 2>&1 | tee ~/laptop.log
```

Optionally, review the log:
```
less ~/laptop.log
```

Once Script Completes
---------------------

In iTerm2:
Preferences > Profile > Command:
Send text at start: `ssh-add -K`

In Terminal, let GitHub know who you are:

```
git config --global user.name "Your Name"
git config --global user.email your.name@spark.re
```

**Important:**
Your computer has been through a lot since running this script.
Restart it and move onto the next step.

Setup Rbenv, Ruby and Spark
---------------------------

```
rbenv install 2.6.5
rbenv global 2.6.5
```

Relaunch your terminal.

```
gem install bundler
gem install rails
rbenv rehash

cd ~/Sites
git clone git@github.com:sprk/spark.git
cd spark
```

Note: If you want Spark to be installed a directory other than 'sites', the following will need to be updated:
```
In itermocil/spark.yml - (code ~/.itermocil/spark.yml)
windows:
  - name: Spark
    root: ~/Sites/spark
```
```
In zshrc - (zshconfig)
# FOLDER ALIASES
alias si='cd ~/Sites/Spark'
```

Create the master key and copy the variable that has been shared with
you in the Dashlane "Sharing Center" under "Rails - Credentials Master Key"
`touch config/master.key && open config/master.key`

Populate your private variables in the zshrc private file:
`open ~/.files/zshrc/private`
`SPARK_SEED_ADMIN_EMAIL="first.lastname@spark.re"`

Unless you have received an aws access key id and secret key, leave these variables as is.

Setup your local database and put your computers username in the designated field.
`cp config/settings.local.example.yml config/settings.local.yml`
`open config/settings.local.yml`

Ensure yarn is setup correctly.
`yarn install --check-files`

Create and Populate your local database:
`rboot`


Debugging
---------

Your last Laptop run will be saved to `~/laptop.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a
[new GitHub Issue](https://github.com/thoughtbot/laptop/issues/new) for us.
Or, attach the whole log file as an attachment.

What it sets up
---------------

macOS tools:

* [Homebrew] for managing operating system libraries.

[Homebrew]: http://brew.sh/

Unix tools:

* [Exuberant Ctags] for indexing files for vim tab completion
* [Git] for version control
* [OpenSSL] for Transport Layer Security (TLS)
* [RCM] for managing company and personal dotfiles
* [The Silver Searcher] for finding things in files
* [RipGrep] as a faster alternative to the Silver Surfer
* [Tmux] for saving project state and switching between projects
* [Watchman] for watching for filesystem events
* [Zsh] as your shell

[Exuberant Ctags]: http://ctags.sourceforge.net/
[Git]: https://git-scm.com/
[OpenSSL]: https://www.openssl.org/
[RCM]: https://github.com/thoughtbot/rcm
[The Silver Searcher]: https://github.com/ggreer/the_silver_searcher
[Tmux]: http://tmux.github.io/
[Watchman]: https://facebook.github.io/watchman/
[RipGrep]: https://github.com/BurntSushi/ripgrep
[Zsh]: http://www.zsh.org/

GitHub tools:

* [Hub] for interacting with the GitHub API

[Hub]: http://hub.github.com/

Image tools:

* [ImageMagick] for cropping and resizing images

Programming languages, package managers, and configuration:

* [Node.js] and [NPM], for running apps and installing JavaScript packages
* [Ruby] stable for writing general-purpose code
* [Yarn] for managing JavaScript packages

[Bundler]: http://bundler.io/
[ImageMagick]: http://www.imagemagick.org/
[Node.js]: http://nodejs.org/
[NPM]: https://www.npmjs.org/
[Ruby]: https://www.ruby-lang.org/en/
[Yarn]: https://yarnpkg.com/en/

Databases:

* [Postgres] for storing relational data
* [Redis] for storing key-value data

[Postgres]: http://www.postgresql.org/
[Redis]: http://redis.io/

Programs:
* [Brave Browser] Like Google Chrome but with built in privacy / ad-blockers.
* [Dashlane] our password management tool.
* [Firefox] always good to have a secondary browser. Plus nice dev tools.
* [iTerm2] your standard terminal... upgraded.
* [Postman] for Spark API queries.
* [PSequel] for checking out your local database.
* [Slack] office communications/chat channels.
* [VSCode] only code editor a sane person will ever need.

[Brave Browser]: https://brave.com/
[Dashlane]: https://www.dashlane.com/
[Firefox]: https://www.mozilla.org/en-CA/firefox/
[iTerm2]: https://iterm2.com/
[Postman]: https://www.getpostman.com/
[PSequel]: http://www.psequel.com/
[Slack]: https://slack.com/intl/en-ca/
[VSCode]: https://code.visualstudio.com/

It should take less than 15 minutes to install (depends on your machine).

Contributing
------------

Edit the `laptop` file.
Document in the `README.md` file.
Follow shell style guidelines by using [ShellCheck] and [Syntastic].

```sh
brew install shellcheck
```

[ShellCheck]: http://www.shellcheck.net/about.html
[Syntastic]: https://github.com/scrooloose/syntastic

Thank you!

Spark fork maintainers (you!) + original [contributors]
[contributors]: https://github.com/thoughtbot/laptop/graphs/contributors

By participating in this project,
you agree to abide by the thoughtbot [code of conduct].

[code of conduct]: https://thoughtbot.com/open-source-code-of-conduct

License
-------

Laptop is © 2011-2018 thoughtbot, inc.
It is free software,
and may be redistributed under the terms specified in the [LICENSE] file.

[LICENSE]: LICENSE

About thoughtbot
----------------

![thoughtbot](http://presskit.thoughtbot.com/images/thoughtbot-logo-for-readmes.svg)

Laptop is maintained and funded by thoughtbot, inc.
The names and logos for thoughtbot are trademarks of thoughtbot, inc.

We are passionate about open source software.
See [our other projects][community].
We are [available for hire][hire].

[community]: https://thoughtbot.com/community?utm_source=github
[hire]: https://thoughtbot.com?utm_source=github
