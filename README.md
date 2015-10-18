# Brain

#React JS
Use this repository instead of your brain. Super helpful?
## Ref (getElementById React Style)
```
<div ref="myExample">
React.findDOMNode(this.refs.myExample)
```
## React Bootstrap
* https://github.com/react-bootstrap/react-bootstrap
* http://react-bootstrap.github.io/

# Amazon Web Services

## Install AWS CLI
```brew install awscli```

[Then configure it (AWS CLI Getting Started)](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)

## DynamoDB
[Tutorial](http://docs.aws.amazon.com/amazondynamodb/latest/gettingstartedguide/Welcome.html)

### Run local DynamoDB
```java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -sharedDb```

## application.yml
```
amazon:
  #The live server location, if you want to run against it
  region: EU_WEST_1
  dynamodb:
    # If this is set, run against dynamo db local
    endpoint: http://localhost:8000/
logging:
  level:
    com:
      package: TRACE
```

# JSON
## Pretty print JSON
pbpaste works by default on OSX, can work on linux if you define it.

### Using underscore
* [Underscore-cli GitHub](https://github.com/ddopson/underscore-cli)

A CLI for parsing JSON data into component parts, usable by your shell.

```
pbpaste | underscore print --color | less
```

### Using python
```
pbpaste | python -m json.tool | less
```

# Gradle

## Force an update of dependencies
```
./gradlew --refresh-dependencies dependencies
```

## Enable gradle daemon
[Docs](https://docs.gradle.org/2.6/userguide/gradle_daemon.html)

Put

```
org.gradle.daemon=true
```

in ~/.gradle/gradle.properties

# IntelliJ

## Enable Annotation processing in IntelliJ
* Build, Execution, Deployment > Compiler > Annotation Processors > Enable annotation processing
* Plugins > Browse repositories > Lombok
* Add @ComponentScan annotation to your @SpringBootApplication, otherwise @Autowired can't be found by IntelliJ (?)

#OSX

## Environment variables
* [Environment variables for non-terminal using plist?](https://codingdaily.wordpress.com/2010/10/28/how-to-edit-macosxenvironment-plist-from-a-shell/)
* Not working since Yosemite? [Environment variables (not in .bashrc)](http://www.dowdandassociates.com/blog/content/howto-set-an-environment-variable-in-mac-os-x-slash-etc-slash-launchd-dot-conf/)
```
#These won't end up in your terminal PATH
defaults write ~/.MacOSX/environment PATH "/all/your/paths/:/paths/"
```

##Improve performance
MacOS X (on mid 2014 rMBP) has pretty bad performance on an external 4k display. These tips might help:
* http://osxdaily.com/2014/10/24/speed-up-os-x-yosemite-mac/

## Tips & Trix
* [iTerm jump and end of line](http://stackoverflow.com/questions/6205157/iterm2-how-to-get-jump-to-beginning-end-of-line-in-bash-shell)
* [Launch sumblime text from terminal](https://gist.github.com/artero/1236170 launch sublime from terminal)

## List of Apps to choose from (just to remember them all)
* iTerm2
 * [Solarized Theme](http://ethanschoonover.com/solarized)
* MacPass (Native KeePass2 for MacOS)
* [Git](http://git-scm.com/) (Follow the instructions!)
 * [Git bash completion](http://code-worrier.com/blog/autocomplete-git/)
* Emacs
* Sublime Text
* TextEdit
* XCode
* [CyberDuck FTP Client](https://cyberduck.io/)
* [Zsh / brew](http://rick.cogley.info/post/use-homebrew-zsh-instead-of-the-osx-default/)
 * [Oh-My-Zsh GitHub](https://github.com/robbyrussell/oh-my-zsh), [http://ohmyz.sh/](http://ohmyz.sh/), [Themes](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)
* Soundflower for MiniDP sound adjustment [This fork seems best right now](https://github
.com/mLupine/SoundflowerBed/releases)
* [OSX Fuse](https://osxfuse.github.io/) Needed by VeraCrypt
* [VeraCrypt](https://veracrypt.codeplex.com/) If you need that sort of thing
* [Kdiff3](http://kdiff3.sourceforge.net/) Don't forget to set it as default mergetool
* [Atom](https://atom.io/) by Github
* [GPGTools](https://gpgtools.org/)
* [RVM](https://rvm.io/rvm/install) Run multiple Ruby versions simultaneously.
* [Ukelele Keyboard Layout Editor](http://scripts.sil.org/ukelele)
* [Funter](https://nektony.com/products/funter) Change hidden files visible / invisible
* [SizeUp](http://www.irradiatedsoftware.com/sizeup/) by IrradiatedSoftware (maximize / center / snapback windows)
* [Dash API lookup](https://kapeli.com/dash)
* [Atlassian SourceTree](http://www.sourcetreeapp.com/)
* [Smooth Mouse](http://smoothmouse.com/) for turning off mouse acceleration
* [Karabiner](https://pqrs.org/osx/karabiner/) for increasing repeat rate and decreasing repeat wait timer
* [Finderpath](https://bahoom.com/finderpath/) (Command+G path in finder)
* [Zipeg](http://www.zipeg.com/) (Zip program, seems slow?)
* Chat apps (Gtalk)
 * Flamingo (19 kr)
 * Adium (Free)
* [Localcast](http://scalableminds.github.io/localcast/#download) Stream video to Chromecast using
* [Hyperswitch](https://bahoom.com/hyperswitch) Command+Tab replacement

[Lifehacker essential mac list](http://lifehacker.com/lifehacker-pack-for-mac-our-list-of-the-essential-mac-635303836)

## Git
Install:
```
brew install git
```

Or if you want to use git with git-extras you should probably use
```
brew install git --without-completions
```

### git-extras
* [Command listing](https://github.com/tj/git-extras/blob/master/Commands.md)

According to README.md in ~/.oh-my-zsh/plugins/git-extras:
On OS X with Homebrew, you need to install `git` with `brew install git --without-completions`. Otherwise, `git`'s `_git` will take precedence, and you won't see the completions for `git-extras` commands.

Then install git-extras itself via `brew install git-extras`

## Monitor bandwidth with bmon
```
brew install bmon
```

## Stay sane
* [Re-map Command/Option key on Windows USB keyboard) (Instead of losing your mind)](http://superuser.com/questions/80976/how-to-re-map-command-and-option-keys-on-mac-os-x-with-a-pc-keyboard)

## Workarounds
### IntelliJ can't find git executable for use in exec()
If you installed git from git-scm.org according to the instructions, you need to symlink /usr/bin/git to /usr/local/git/bin/git otherwise intellij can't find git in exec().

### Key repeat not working in some apps (Lion)
Reenable key repeat in some apps, reboot needed:
```defaults write -g ApplePressAndHoldEnabled -bool false```


## Potentially useful
* [JEnv](http://www.jenv.be/) Set Java Home, locally for a given path if you want.

## Launch an app from terminal
Example with Atlassian SourceTree
```
open -a SourceTree <path/to/repository>
```

## File system tracing OSX
[Use Instruments](http://apple.stackexchange.com/questions/14409/how-to-monitor-file-access-for-an-os-x-application)
```
/Applications/Xcode.app/Contents/Applications/Instruments.app
```

#Windows

* [GPG4Win](http://www.gpg4win.org/)
* [Kdiff3](http://kdiff3.sourceforge.net/)
* [Git](http://git-scm.com/)
* [Sublime](http://www.sublimetext.com/)

# Zsh
## Enable / disable function tracing
```
setopt xtrace
my_func
unsetopt xtrace
```
