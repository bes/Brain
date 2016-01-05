# Brain

Use this repository instead of your brain. Super helpful?





[](================================================================================================================)
# React

## Ref (getElementById React Style)
```
<div ref="myExample">
React.findDOMNode(this.refs.myExample)
```
## React Bootstrap
* https://github.com/react-bootstrap/react-bootstrap
* http://react-bootstrap.github.io/





[](================================================================================================================)
# Webpack

## Hot reloading
* [Webpack dev server](http://webpack.github.io/docs/webpack-dev-server.html)
* [Geowarin guide to react + spring (with proxy) hot reloading](http://geowarin.github.io/spring-boot-and-react-hot.html)
* [Node Proxy (options)](https://github.com/nodejitsu/node-http-proxy)





[](================================================================================================================)
# GPG
## Encrypt to a specific person
### Encrypt
```
pbpaste | gpg -ear <recipient>
```
### Decrypt
```
pbpaste | gpg -d
```
## Encrypt with a password
### Encrypt
```
gpg --sign -c file.txt
```
### Decrypt
```
gpg --output <file> -d <file>.gpg
```





[](================================================================================================================)
# OpenSSL
* [Answer on stackoverflow](http://stackoverflow.com/questions/16056135/how-to-use-openssl-to-encrypt-decrypt-files)
## Encrypt with password
```
openssl aes-256-cbc -a -salt -in <INFILE> -out <OUTFILE>
```

## Decrypt with password
```
openssl aes-256-cbc -d -a -in <INFILE> -out <OUTFILE>
```

## Options
* -a Means that the file (in/out) should be in Base64
* -d Means decrypt





[](================================================================================================================)
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
  dynamoDb:
    # If this is set, run against dynamo db local
    endpoint: http://localhost:8000/
logging:
  level:
    com:
      package: TRACE
```





[](================================================================================================================)
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





[](================================================================================================================)
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





[](================================================================================================================)
# Zsh
## Enable / disable function tracing
```
setopt xtrace
my_func
unsetopt xtrace
```




[](================================================================================================================)
# Git
## List changes
```
git log --oneline --graph origin/master..HEAD
```

## Tag a version
```
git tag -a <tagname> [<commit>]
git push <remote> --tags
```

## Push the contents of a new branch
```
git push <remote> HEAD:refs/for/branch-name
```

## List all local branches with tracking information
```
git branch -lvv
```

## Find a changed row
```
git log --patch | grep -E "(commit ([0-9]|[a-fA-F])+|((\+|\-).+(returnData)))" | less
```

In a specific file
```
git log --patch ./path/to/File.java | grep -E "(commit ([0-9]|[a-fA-F])+|((\+|\-).+(returnData)))" | less
```

## Find a patch that contains specific words
```
git log -Sword
```
[See answer on stack overflow](http://stackoverflow.com/questions/1337320/how-to-grep-git-commits-for-a-certain-word)

## Create a topic branch from a remote branch with tracking
```
git branch --track topic_branch_name origin/rel-7.2.A.0
```

## Git subtree
[Copied from this article](http://blogs.atlassian.com/2013/05/alternatives-to-git-submodule-git-subtree/)
First add a remote
```
git remote add -f <remote name> <git path>
```

Then add the subtree
```
git subtree add --prefix <new path of subtree> <remote> master --squash
```

Update the subtree later
```
git fetch <remote> master
git subtree pull --prefix <path of subtree> <remote> master --squash
```

Contribute to upstream
```
git subtree push --prefix=<path of subtree> <remote> master
```


[](================================================================================================================)
# Atom Editor
## Essential packages
* https://atom.io/packages/ctrl-last-tab
* https://atom.io/packages/merge-conflicts





[](================================================================================================================)
# IntelliJ
## Enable Annotation processing in IntelliJ
* Build, Execution, Deployment > Compiler > Annotation Processors > Enable annotation processing
* Plugins > Browse repositories > Lombok
* Add @ComponentScan annotation to your @SpringBootApplication, otherwise @Autowired can't be found by IntelliJ (?)

# GPG
* [GPG Mini Howto](http://www.dewinter.com/gnupg_howto/english/GPGMiniHowto-3.html)
* [What default algorithm to use (See top answer)]( http://superuser.com/questions/541090/best-encryption-and-signing-algorithm-for-gnupg-rsa-rsa-or-dsa-elgamal) I use RSA / RSA.

# Logging
## Cleaner log using sed
```
adb logcat | grep SomeTerm | sed -e 's/\/SomeOtherTerm[(].....[)]//'
```

## Redirect std out & std err to /dev/null
```
command > /dev/null 2>&1
```

## Redirecting std out & std err to two different files and seeing the output at the same time
```
command > >(tee ~/Desktop/buildlog/stdout.log) 2> >(tee ~/Desktop/buildlog/stderr.log >&2)
```





[](================================================================================================================)
# Android
## adb input shell script
I usually call it `ainput`
```
#!/bin/bash
#space is %s, "" is empty string, on bash command line
for var in "$@"
do
    if [ "$var" != "" ]; then
        adb shell input text $var
    fi
    adb shell input keyevent KEYCODE_DPAD_DOWN
done
```

## Android APK debugging
Check for resources inside APK
~/sdk/android-curr/build-tools/21.1.1/aapt dump resources build/outputs/apk/MyApk.apk | grep raw/

## Android System / Apps

### get package name / apk location
```
adb shell pm list packages -f
```

### get apk Manifest main info
```
aapt d badging <APK>
```

### Monkey
```
adb shell monkey -s 20120614 -v -p com.package.name -p com.package2.name --pct-touch 20 --pct-motion 60 --pct-majornav 15 --pct-appswitch 5 --throttle 300 1000000 | tee log.txt
```

### Systrace
```
~/sdk/android-curr/platform-tools/systrace/systrace.py --app=com.package.name gfx input view wm am hal res dalvik sched freq idle load
```

### Memory usage for a process
```
$ adb shell dumpsys meminfo com.package.name
```
or
```
adb shell procrank | grep home
```

* https://developer.android.com/tools/debugging/debugging-memory.html
* Proportional Set Size (PSS) = Unique pages + Fraction of Shared pages
* Private (Clean and Dirty) RAM
 * Private dirty ram = Used only by your process, only exists in RAM

## Force backup of a specific package (for testing)
Result of operation (e.g. encrypted content) is output to FD 0 (stdout) = shown in terminal.
```
$ adb shell bu 0 backup <package name>
```
More options are available in `frameworks/base/cmds/bu/src/com/android/commands/bu/Backup.java`

### tcpdump
```
$ tcpdump -i <interface> -s 65535 -w <some-file>
$ tcpdump -i any -p -s 0 -w /sdcard/capture.pcap
# "-i any": listen on any network interface
# "-p": disable promiscuous mode (doesn't work anyway)
# "-s 0": capture the entire packet
# "-w": write packets to a file (rather than printing to stdout)
```

http://www.kandroid.org/online-pdk/guide/tcpdump.html

Only for loopback:
```
$ adb shell "tcpdump -i lo  -vv -s 0 -w /sdcard/tcpoutput.pcap"
```

## Android Screens and resolution
DPI Scale factors
* LDPI = 0.75
* MDPI = 1
* TVDPI = 1.33
* HDPI = 1.5
* XHDPI = 2
* XXHDPI = 3

### System Screen Density
#### Density
`$ adb shell getprop ro.sf.lcd_density`

#### Screen width
`$ adb shell dumpsys window | grep DisplayWidth`

#### Simulate screen density and resolution
Ex. HVGA
```
$ adb shell wm size 320x480
$ adb shell wm density 160
```

Ex. Tablet
```
$ adb shell wm size 1080x1920
$ adb shell wm density 240
```

Tip: kill com.android.systemui from DDMS to force it to load the correct resources.

## SQLite extra logging
```
$ adb shell setprop log.tag.SQLiteLog V
$ adb shell setprop log.tag.SQLiteStatements V
$ adb shell stop
$ adb shell start
```

## Device emulator with latency/delay
```
emulator -avd <some avd> -netdelay 400:1200 -netspeed gsm
```
* [Android: Emulator](http://developer.android.com/tools/help/emulator.html)
* [Android: Emulator Netdelay](http://developer.android.com/tools/devices/emulator.html#netdelay)
* [Android: Emulator Netspeed](http://developer.android.com/tools/devices/emulator.html#netspeed)





[](================================================================================================================)
# Un*x Common
## make a random big file
```
$ sudo dd if=/dev/urandom of=/media/696fc5a9-a903-43d3-ae99-f84f559d152c/bigfile bs=1M count=458000
#poll status of running dd
$ sudo kill -USR1 `pidof dd`
```

## Loop over a number of files and execute a command for each one
`$ for f in $(find -name "*.apk"); do adb install -r $f; done`

## Remove CRLF at the end of lines in a file
Use dos2unix command in ubuntu





[](================================================================================================================)
# IOS
* Cocoapods dependency management https://cocoapods.org/
* [Swift examples](Swift.md)





[](================================================================================================================)
# Testing network conditions using WiFi
* [How to test your software under bad network conditions](NetworkConditionsTesting.md)





[](================================================================================================================)
# Python
Download [Miniconda from here](http://conda.pydata.org/miniconda.html), don't forget to add conda to your PATH.

## Use Conda
```bash
#Create a new sandbox
conda create -n my_sandbox python=3.5

#Use the sandbox in this terminal
source activate my_sandbox

#iPython is probably useful
conda install ipython
```

## Use iPython
```python
$ ipython
from webmonkey import monkey
mon = monkey.SW()
mon.driver...
```





[](================================================================================================================)
# Virtual machine using Vagrant & VirtualBox
[Vagrant](https://www.vagrantup.com/) is an easy way to set up virtual machines. Don't forget to install [VirtualBox](https://www.virtualbox.org/).
## Vagrantfile example
Place in ~/vagrant/Vagrantfile
```
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
 # The most common configuration options are documented and commented below.
 # For a complete reference, please see the online documentation at
 # https://docs.vagrantup.com.

 # Every Vagrant development environment requires a box. You can search for
 # boxes at https://atlas.hashicorp.com/search.
 config.vm.box = "ubuntu/trusty64"

 # Create a forwarded port mapping which allows access to a specific port
 # within the machine from a port on the host machine. In the example below,
 # accessing "localhost:8080" will access port 80 on the guest machine.
 config.vm.network "forwarded_port", guest: 80, host: 8000
 #config.vm.network "forwarded_port", guest: 9001, host: 9001

 # Create a private network, which allows host-only access to the machine
 # using a specific IP.
 # config.vm.network "private_network", ip: "192.168.33.10"

 # Share an additional folder to the guest VM. The first argument is
 # the path on the host to the actual folder. The second argument is
 # the path on the guest to mount the folder. And the optional third
 # argument is a set of non-required options.

 # This assumes that binary_tools and camera are checked out in the same
 # directory.
 #config.vm.synced_folder "/Users/<user>/Downloads", "/downloads"

 # Enable provisioning with a shell script. Additional provisioners such as
 # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
 # documentation for more information about their specific syntax and use.
 #config.vm.provision "shell", inline: <<-SHELL
 #SHELL
end
```
## Vagrant up
To start the machine in Vagrantfile do
```
vagrant up
```
## Vagrant ssh
To enter the machine do
```
vagrant ssh
```
## More commands
```
#Save the current state and stop
vagrant suspend

#Shut down the OS and power down the guest machine
vagrant halt

#Completely remove the machine from the host
vagrant destroy
```





[](================================================================================================================)
# OSX
## Environment variables
* [Environment variables for non-terminal using plist?](https://codingdaily.wordpress.com/2010/10/28/how-to-edit-macosxenvironment-plist-from-a-shell/)
* Not working since Yosemite? [Environment variables (not in .bashrc)](http://www.dowdandassociates.com/blog/content/howto-set-an-environment-variable-in-mac-os-x-slash-etc-slash-launchd-dot-conf/)
```
#These won't end up in your terminal PATH
defaults write ~/.MacOSX/environment PATH "/all/your/paths/:/paths/"
```

## Find the program running on a port
```
sudo lsof -i :8080
```

##Improve performance
MacOS X (on mid 2014 rMBP) has pretty bad performance on an external 4k display. These tips might help:
* http://osxdaily.com/2014/10/24/speed-up-os-x-yosemite-mac/

## Tweaks
* [Global shortcut for dark/light mode](http://www.tekrevue.com/tip/os-x-yosemite-dark-mode-shortcut/) Control+Option+Command+T.
* [iTerm jump and end of line](http://stackoverflow.com/questions/6205157/iterm2-how-to-get-jump-to-beginning-end-of-line-in-bash-shell)
* [Launch sumblime text from terminal](https://gist.github.com/artero/1236170 launch sublime from terminal)


### Copy public key
```
pbcopy < ~/.ssh/id_rsa.pub
```

## List of OSX Apps
### Essential
* [Homebrew](http://brew.sh/) - brew for short.
 * [Homebrew FAQ](https://github.com/Homebrew/homebrew/blob/master/share/doc/homebrew/FAQ.md)
* iTerm2
* [Zsh / brew](http://rick.cogley.info/post/use-homebrew-zsh-instead-of-the-osx-default/)
 * [Oh-My-Zsh GitHub](https://github.com/robbyrussell/oh-my-zsh), [http://ohmyz.sh/](http://ohmyz.sh/), [Themes](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)
* Git - install using brew
 * [Kdiff3](http://kdiff3.sourceforge.net/) For git-mergetool ([Set-up](http://stackoverflow.com/questions/9776434/git-mergetool-config-on-mac-osx))
   * git config --global merge.tool kdiff3
   * git config --global mergetool.kdiff3.path /Applications/kdiff3.app/Contents/MacOS/kdiff3
   http://stackoverflow.com/questions/9776434/git-mergetool-config-on-mac-osx
* [MacPass](https://github.com/mstarke/MacPass) (Native KeePass2 for MacOS)
* [Hyperswitch](https://bahoom.com/hyperswitch) Command+Tab replacement
* [Smooth Mouse](http://smoothmouse.com/) for turning off mouse acceleration
* [Karabiner](https://pqrs.org/osx/karabiner/) for increasing repeat rate and decreasing repeat wait timer
* [Atom](https://atom.io/) Competent text editor by Github
* Sublime Text
* [SizeUp](http://www.irradiatedsoftware.com/sizeup/) by IrradiatedSoftware (maximize / center / snapback windows)
* [iStat Menus](https://bjango.com/mac/istatmenus/) Some useful status bar menus (CPU, Network, Calendar)
* XCode
* [RVM](https://rvm.io/rvm/install) Run multiple Ruby versions simultaneously.

### Optional
* [XnViewMP](http://www.xnview.com/) image viewer / editor
* [CyberDuck FTP Client](https://cyberduck.io/)
* Soundflower advanced sound input/output adjustment [This fork seems best right now](https://github
.com/mLupine/SoundflowerBed/releases)
* [VeraCrypt](https://veracrypt.codeplex.com/) If you need that sort of thing
 * [OSX Fuse](https://osxfuse.github.io/) Needed by VeraCrypt
 * [GPGTools](https://gpgtools.org/)
* [Ukelele Keyboard Layout Editor](http://scripts.sil.org/ukelele)
* [Funter](https://nektony.com/products/funter) Change hidden files visible / invisible
* [Dash API lookup](https://kapeli.com/dash)
* [Atlassian SourceTree](http://www.sourcetreeapp.com/)
* [Finderpath](https://bahoom.com/finderpath/) (Command+G path in finder)
* [Zipeg](http://www.zipeg.com/) (Zip program, seems slow?)
* Chat apps (Gtalk)
 * Adium (Free, uglier than Flamingo but has better notifications)
 * Flamingo (19 kr)

[Lifehacker essential mac list](http://lifehacker.com/lifehacker-pack-for-mac-our-list-of-the-essential-mac-635303836)

## Git for OSX
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

## Monitor network / bandwidth with bmon
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





[](================================================================================================================)
#Windows

* [GPG4Win](http://www.gpg4win.org/)
* [Kdiff3](http://kdiff3.sourceforge.net/)
* [Git](http://git-scm.com/)
* [Sublime](http://www.sublimetext.com/)
* [Chocolatey Windows Package Manager](https://chocolatey.org/)




[](================================================================================================================)
# Linux
## Java alternatives
Used to select what java versions are used, on Ubuntu Linux (probably other dists as well)
```
$ sudo update-alternatives --config java
$ sudo update-alternatives --config javac
$ sudo update-alternatives --config jar
$ update-java-alternatives -l
```

## Ubuntu 12.04
* VirtualBox
 * [Latest VirtualBox as package on Ubuntu 12.04](http://linuxg.net/how-to-install-virtualbox-4-3-on-ubuntu-13-04-12-10-12-04-linux-mint-15-14-13-pear-os-8-pear-os-7-and-elementary-os-0-2-via-the-official-virtualbox-repository/)
 * Resize virtualbox
  * [Method 1](http://blog.rmartinez.co/2013/01/tutorial-resize-static-virtualbox-vdis.html)
  * Method 2:
  ```
$ vboxmanage createhd --size 40960 --variant Fixed --filename new.vdi
$ vboxmanage clonehd old.vdi new.vdi –existing
Then, inside windows: extend partition.
Or, [for dynamic size disks](https://brainwreckedtech.wordpress.com/2012/01/08/howto-convert-vdis-between-fixed-sized-and-dynamic-in-virtualbox/)
$ vboxmanage modifyhd [VDI] --resize [megabytes]
Then, inside windows: extend partition.
  ```
* Chrome
 * [Latest Chrome as package](http://www.howopensource.com/2011/10/install-google-chrome-in-ubuntu-11-10-11-04-10-10-10-04/)
 * [Chromium policy settings](http://www.chromium.org/administrators/linux-quick-start)
  * In chrome browser: `chrome://policy/` to check policies
* Theming
 * Manually fix any theme [Tooltip background color Eclipse](http://askubuntu.com/questions/70599/how-to-change-tooltip-background-color-in-unity)
 * Or just use [Clearlooks for GTK3](http://www.jpfleury.net/en/software/clearlooks-phenix.php)
* [Java 7 Sun](http://www.webupd8.org/2012/01/install-oracle-java-jdk-7-in-ubuntu-via.html)
* System settings
 * Use /etc/environment for "global" variables
* Gnome
 * Create links using the app "alacarte"
   * Doesn't work out-of-the-box, see: http://askubuntu.com/questions/160462/why-does-alacarte-give-me-this-error-when-i-try-to-add-items-to-the-menu You need to install the package gnome-panel.
 * Ubuntu 12.04
   * Calendar widget with week numbers http://askubuntu.com/questions/114032/ubuntu-application-to-show-calendar-with-week-numbers
 * Gnome
   * Use > Window List Extension, traytop extension (not so important: Native Window Placement Extension, overview-icon Extension)
   * Gnome 3 keyboard shortcuts: dconf-editor guide: http://askubuntu.com/questions/135425/keyboard-shortcuts-dont-work-in-gnome-shell
   * Gnome 3 TrayTop [gnome extensions github](https://extensions.gnome.org/extension/712/traytop/) (use with blink in pidgin + don't always show icon in tray)
   * Gnome 3 Overlay icons [github](https://github.com/sustmi/gnome-shell-extensions-sustmi)
   * Gnome 3 WindowList [github](https://github.com/siefkenj/gnome-shell-windowlist)
   * Gnome 3 Shell Extension dir ~/.local/share/gnome-shell/extensions/extension@owner.com
   * [Install Gnome 3 on 12.04](http://www.filiwiese.com/installing-gnome-on-ubuntu-12-04-precise-pangolin/)
   * [How to set location bar in nautilus as default](http://www.omgubuntu.co.uk/2011/11/how-to-set-location-bar-in-nautilus-as-default)
   * [Compiz forgets your settings (keyboard shortcuts, change preferences to "Flat file"](https://bugs.launchpad.net/ubuntu/+source/compiz/+bug/964270)
   * Scrollbars http://www.linuxbsdos.com/2012/04/27/whats-the-point-of-having-2-scrollbar-types-in-ubuntu-12-04/ and http://ubuntuforums.org/showthread.php?t=1949608
   * Show date in clock widget http://forums.fedoraforum.org/showthread.php?t=261961
* Handbrake http://askubuntu.com/questions/107915/how-do-i-download-and-install-handbrake
