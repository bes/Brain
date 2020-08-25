# Brain

Use this repository instead of your brain. Super helpful?




# SSH

## configure multiple ssh keys for Github

```
Host github1
   HostName     github.com
   User         git
   IdentityFile ~/.ssh/id_rsa_github1
Host github2
  HostName     github.com
  User         git
  IdentityFile ~/.ssh/id_rsa_github2
```

Then register additional ssh keys using ssh-add

```
ssh-add -t 12h ~/.ssh/your_private_key
```

List current keys using

```
ssh-add -l
```

The last step is to clone the git using the new virtual Host from the ssh config

```
git clone git@github1:username/projectname.git
```

## Configure ssh through another ssh proxy (use with e.g. AWS)

```
#The ssh proxy machine
Host my-proxy-name
  HostName     <ip address>
  User         ec2-user
  IdentityFile ~/.ssh/<identity>.pem

#Connect to a host matching a pattern
Host <partial-match-dns>-*.eu-west-1.compute.internal
  User         ec2-user
  IdentityFile ~/.ssh/<identity>.pem
  ProxyCommand ssh my-proxy-name -W %h:%p

#Connect to a specific host through a proxy machine running ssh
Host <some-other-host-trivial-name>
  User         ubuntu
  IdentityFile ~/.ssh/spiideo-kp1.pem
  ProxyCommand ssh my-proxy-name -W <some-specific-machine>.eu-west-1.compute.internal:22
```


## Easy "VPN" tunnel using SSH

* Download / install [sshuttle](https://github.com/apenwarr/sshuttle/)
* `sshuttle --dns -r username@remote-server-ip-or-name 0/0`






[](================================================================================================================)

# Network analysis

Tools for analyzing local networks

## Primary WAN interface:

```
ip route get 8.8.8.8
```

## List SSIDs

```
iw dev wlan0 scan | grep "SSID:" | sort | uniq
```

## IP neighbors

List local ip addresses that were recently connected.

This is primarily run FROM a router, e.g. OpenWRT or other router that has a shell.

```
ip neigh
```

## mDNS

```
dns-sd -B _http
dns-sd -L "Instance name here" _http._tcp.
dns-sd -Gv4v6 bla-bla-bla.local
```

## Port scan

Scan all ports for a specific IP address

```
nmap -p- 192.168.1.2
```

TODO: Add more nmap examples








[](================================================================================================================)
# React

## Guides

* [Pass props to children](https://frontarm.com/james-k-nelson/passing-data-props-children/)








[](================================================================================================================)

# Webpack

## Hot reloading

* [Webpack dev server](http://webpack.github.io/docs/webpack-dev-server.html)
* [Geowarin guide to react + spring (with proxy) hot reloading](http://geowarin.github.io/spring-boot-and-react-hot.html)
* [Node Proxy (options)](https://github.com/nodejitsu/node-http-proxy)

## Size report for package

Install the webpack size analyzer

```
npm install -g webpack-bundle-size-analyzer
```

Then analyze your bundle:

```
./node_modules/.bin/webpack --json | webpack-bundle-size-analyzer
```

N.B: You may not output anything using console.log from your script when analyzing, since
`webpack-bundle-size-analyzer` will break down.



[](================================================================================================================)

# GPG

## Encrypt to a specific person

### From clipboard

#### Encrypt

```
pbpaste | gpg -ear <recipient>
```

#### Decrypt

```
pbpaste | gpg -d
```

### From file

#### Encrypt

```
gpg --output encrypted_file.gpg --encrypt --recipient <recipient> file_to_encrypt
```

#### Decrypt

```
gpg --output decrypted_file --decrypt encrypted_file.gpg
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
openssl aes-256-cbc -a -salt -in <INFILE> -out <OUTFILE>.openssl
```

## Decrypt with password

```
openssl aes-256-cbc -d -a -in <INFILE>.openssl -out <OUTFILE>
```

## Options

* -a Means that the file (in/out) should be in Base64
* -d Means decrypt





[](================================================================================================================)
# SSH

## Port forwarding from a remote server port to a local port

```
# ssh -N -L [bind_address:]<local port>:<host>:<remote port> <server>
ssh -N -L 5005:localhost:4000 some.ssh.server.com
```












[](================================================================================================================)
# Rust

Zsh autocompletion and other things are setup in [`.zshrc`](https://github.com/bes/Betup/blob/master/Zsh/dotzshrc).

## Rustup

* `rustup update`
* `rustup show`
* `rustup toolchain list`
* `rustup default nightly`
* `rustup default stable`

## Cargo

List installed applications

```
cargo install --list
```

### cargo sweep

See `rustclean` alias in [`.zshrc`](https://github.com/bes/Betup/blob/master/Zsh/dotzshrc) for usage.

### cargo-update

See `rustcargoupdate` alias in [`.zshrc`](https://github.com/bes/Betup/blob/master/Zsh/dotzshrc) for usage.


## Resources

* [The Little Book of Rust Macros](https://danielkeep.github.io/tlborm/book/README.html)







[](================================================================================================================)
# Golang

## Install Using Homebrew

```
brew install go --cross-compile-common
```

## GORE Go REPL

[Golang REPL](https://github.com/motemen/gore)

A good use of gore might be to get current epoch nanos

```
$ gore
:import time
time.Now().UnixNano()
```











[](================================================================================================================)
# Java

## macOS installation alternatives

* Homebrew: `brew cask install adoptopenjdk11`
* SDKMAN!
    * List: `sdk list java`
    * Install: `sdk install java 13.0.1.hs-adpt`

## Measure Heap space graphically

Use [VisualVM](https://visualvm.github.io/)

## Stack traces of running java process

```
pkill -3 java
```

Stacktrace will end up in `/var/log/<some name>.log`

Use `top` and press Shift+H to see thread PIDs, convert them to 0xHex and check against the log.

## Start an instance with debugging

```
java -Xdebug -Xrunjdwp:server=y,transport=dt_socket,address=4000,suspend=n -jar <myapp>.jar
```

* [More info](http://stackoverflow.com/questions/975271/remote-debugging-a-java-application)

See SSH section for port forwarding from a remote server





[](================================================================================================================)
# PHP

Crontab for removing old sessions (if your inode count goes through the roof, and you can't use your system anymore...)

```
0 5  *   *   *     find /var/lib/php5 -name "sess_*" -cmin +24 | xargs rm
```

[Here's a post on stackoverflow for finding the place with the most files](http://stackoverflow.com/questions/653096/howto-free-inode-usage) if you run out of inodes.

Check your inode count using

```
df -i
```






[](================================================================================================================)

# Spring (Boot)

* [Reference](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)
* [API docs](http://docs.spring.io/spring-boot/docs/current/api/)
* [Spring boot with Docker](https://spring.io/guides/gs/spring-boot-docker/)


## Spring (Boot) SYSK

* https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/
    * E.g. Jackson settings are explained under "Customize the Jackson ObjectMapper"
* https://spring.io/blog/2013/05/11/content-negotiation-using-spring-mvc

### Spring errors

* `ErrorMvcAutoConfiguration` - Whitelabel error page configuration
* `DefaultErrorAttributes` - this is (one of?) the place the map of error values are generated
* `ErrorViewResolver` - implement this and install it as a Bean/Component to override the whitelabel error page.






[](================================================================================================================)
# Docker

## List images

`docker images`

## List containers

`docker ps`

Or to show all containers

`docker ps -a`

## Run a container

* `-e`: supply environment variable
* `-it`: interactive shell, if necessary
* `--rm`: means temporary container, will be removed after exit
* `-p`: map port from `HOST:CONTAINER`
* `-d`: Detach - Run container in background and print container ID
* `-v`: Mount a directory in the container `HOST:CONTAINER`
* `--network`: Specify the network, use `host` for running the container in the host network space.
    * On mac the host can be connected to using DNS `docker.for.mac.localhost`

Examples

* `docker run -it --network host --rm -v "$HOME/slask":/opt/slask ubuntu:17.10 bash`
* `docker run -e SOME_ENV_VARIABLE='true' --rm -it -p 8080:8080 name/of/image:version`


## Remove orphaned / dangling volumes

### List them

```
docker volume ls -qf dangling=true
```

### Remove them

```
docker volume rm $(docker volume ls -qf dangling=true)
```


## Simple Docker UI

An app for managing Docker images locally: [Simple Docker UI](https://github.com/felixgborrego/simple-docker-ui)




[](================================================================================================================)

# Google

## Google Sign In

* [Authenticate with a backend server](https://developers.google.com/identity/sign-in/android/backend-auth#send-the-id-token-to-your-server) (Google Sign in for Android)





[](================================================================================================================)
# Amazon Web Services
## Install AWS CLI

```
brew install awscli
```

[Then configure it (AWS CLI Getting Started)](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)

## Selecting which user should be used by AWS CLI

```
export AWS_DEFAULT_PROFILE=user2
```

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

## Additional command line properties for gradle

Additional properties are given on the command line as

```
./gradlew task1 task2 task3 '-PmyPropertyName=some-value'
```

The properties can be used in gradle tasks/plugins/etc:

```
project.findProperty("myPropertyName")
```

## Debug gradle tasks in IntelliJ

```
./gradlew --no-daemon -Dorg.gradle.debug=true <task-name>
```

Then in IDEA create a "Remote" configuration on port 5005 and press debug.

## Force an update of dependencies

```
./gradlew --refresh-dependencies dependencies
```

## Create wrapper

```
gradle wrapper --gradle-version 3.1
```

## Gradle properties

The following options can be put in a `gradle.properties` file in the root of your Gradle project.

```
# Enable gradle daemon
org.gradle.daemon=true

# Enable caching
org.gradle.caching=true

# Set JVM args, e.g. increase heap or set OOM flags
org.gradle.jvmargs=-Xmx4096m -XX:MaxMetaspaceSize=1024m -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/Users/bes/slask/heapdump
```

## Test heap

Configure the test heap

```
allprojects { project ->
    project.plugins.withId('java') {
        // Only for tests
        test {
            maxHeapSize = "1536M"
            jvmArgs "-XX:MaxMetaspaceSize=512m"
        }
    }
    project.plugins.withId('java-library') {
        // Only for tests
        test {
            maxHeapSize = "1536M"
            jvmArgs "-XX:MaxMetaspaceSize=512m"
        }
    }
}
```


## Incredible gradle documents

[Discovering Gradle build scripts from classpath](http://jdpgrailsdev.github.io/blog/2014/07/22/gradle_build_scripts_classpath.html) (embed gradle scripts in gradle plugin)
[Distribute Custom Gradle (wrapper with scripts?)](http://mrhaki.blogspot.se/2012/10/gradle-goodness-distribute-custom.html)
[Provided scope in gradle](https://sinking.in/blog/provided-scope-in-gradle/), use provided scope without dependencies ending up in the distribution)





[](================================================================================================================)
# GCloud

[Installation instructions](https://cloud.google.com/sdk/downloads)

Installing on OSX

```
curl https://sdk.cloud.google.com | bash
```

Using a preview command

```
gcloud preview app deploy
```

## Kubernetes

Install kubectl `gcloud components install kubectl`

### Debug Java

```
-Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=33777 -Dcom.sun.management.jmxremote.rmi.port=33777 -Djava.rmi.server.hostname=127.0.0.1 -Dcom.sun.management.jmxremote.local.only=false -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false
```






[](================================================================================================================)
# Shells

Restart shell

```
exec -l $SHELL
```

## Zsh/Bash keyboard commands

Tables generated with [Tables Generator](http://www.tablesgenerator.com/markdown_tables#)

Cut / Paste in terminal (Readline Kill / Yank)

| Keys | Command       |
|------|---------------|
| C-k  | Readline kill |
| C-y  | Readline yank |

More cutting

| Keys       | Command                             |
|------------|-------------------------------------|
| Alt-d      | Cut forward to end of word          |
| Ctrl-w     | Cut backwards to beginning of word  |
| Ctrl-u     | Cut backwards to beginning of line  |
| Ctrl-space | Set mark (start selecting)          |
| Ctrl-2     | Set mark (same as Ctrl-space)       |
| Alt-w      | Cut marked area                     |
| Ctrl-x-x   | Start marking in opposite direction |


Search backwards / forwards

| Keys | Command          |
|------|------------------|
| C-r  | Search backwards |
| C-s  | Search forwards  |


Less commands

| Keys     | Command                |
|----------|------------------------|
| Spacebar | Next page              |
| b        | Previous page          |
| /<text>  | Forward search <text>  |
| ?<text>  | Backward search <text> |
| n        | Next search match      |
| N        | Previous search match  |
| q        | Quit                   |




[](================================================================================================================)
# Snippets

## List all changed git files and remove the first line of each file if the first line is a newline

```
git ls-files --modified | gxargs gsed -i '1{/^$/d}'
```

* http://stackoverflow.com/questions/11226938/delete-first-line-of-file-if-its-empty

## List directories of that are about 1 GB or larger in size

```
du -h -d1 | ggrep -E "[0-9]+\.?[0-9]*G"
```

## List the 5 largest files in all subdirectories

(macOS, or linux without g prefix)

```
gfind -type f -exec gdu -Sh {} + | gsort -rh | head -n 5
```





[](================================================================================================================)
# Zsh
[My self-answered stackoverflow/superuser question](http://superuser.com/questions/987132/zsh-git-tab-autocomplete-not-working-if-head-is-next-to-last-word-on-input)

## Enable / disable function tracing

```
setopt xtrace
my_func
unsetopt xtrace
```

It's also possible to use (and combined with xtrace):

```
setopt verbose
my_func
unsetopt verbose
```

## Predefined zsh functions

* OSX: `/usr/local/share/zsh/functions`

## oh-my-zsh plugins

`~/.oh-my-zsh/plugins`






[](================================================================================================================)
# Git

## Git configuration

Tell Git not to guess, but rather insist that you set user.name and user.email explicitly before it will let you commit:

```
git config --global --add user.useConfigOnly true
```

## Git push a specific commit to a specific branch

If the branch exists

```
git push <remotename> <commit SHA>:<remotebranchname>
```

If the branch does not yet exist

```
git push <remotename> <commit SHA>:refs/heads/<remotebranchname>
```

[Source](https://stackoverflow.com/a/3230241/816017)


## Git worktree

[Git worktree](https://github.com/blog/2042-git-2-5-including-multiple-worktrees-and-triangular-workflows)

```
git worktree add -b hotfix ../hotfix origin/master
```

When you are done with a linked working tree you can simply delete it

```
rm -rf ../hotfix
```

Administrative files will get gc'd, but you can do that manually too:

```
git worktree prune
```

## Cleaning up

```
git remote prune origin
# Warning will remove all 'newly created' branches as well (removes all fully merged branches from the current branch's perspective)
git branch --merged | grep -v "\*" | grep -v master | grep -v dev | xargs -n 1 git branch -d
```

[See stackoverflow](http://stackoverflow.com/questions/6127328/how-can-i-delete-all-git-branches-which-have-been-merged)

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
git log -Gregex
#or
git log -Sword
```

Print the diff of each file matching the search:
```
git log -Sword -p
```

[See answer on stack overflow](http://stackoverflow.com/questions/1337320/how-to-grep-git-commits-for-a-certain-word)

* [Read this for elaboration on subtleties](http://stackoverflow.com/questions/1337320/how-to-grep-git-commit-diffs-or-contents-for-a-certain-word/1340245#1340245)

## Create a topic branch from a remote branch with tracking

```
git branch --track topic_branch_name origin/rel-7.2.A.0
```

## Add tracking information to an existing branch

```
git branch -u upstream/foo
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
git subtree push --prefix=<path of subtree> <remote> <TARGET-BRANCH>
```





[](================================================================================================================)
# SQL

## macOS

* DB Browser for SQLite
* Sequel Pro






[](================================================================================================================)
# StackOverflow

* [Favorites](http://stackexchange.com/users/431745/erik-z?tab=favorites)






[](================================================================================================================)
# Atom Editor

## Essential packages

* https://atom.io/packages/ctrl-last-tab
* https://atom.io/packages/merge-conflicts





[](================================================================================================================)
# C(++)

## GLFW

You need to install CMake first! Instruction for Linux/OSX:

* [Clone GLFW from GitHub](https://github.com/glfw/glfw)
* `cmake .`
* `make install`

Now you can `#include <GLFW/glfw3.h>`

#ImGui

Framework for menus / windows in OpenGL (e.g. using GLFW)

* [Clone ImGui from GitHub](https://github.com/ocornut/imgui)
d


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

## Flashing CM

* Find model files, download recovery & CM SW
* Go to device Wiki & follow instructions

## Assuming fs privileges for dev app: "run-as"

```
adb shell
run-as com.your.packagename
cd /data/data/com.your.pacakagename/
```

[See this answer on stackoverflow](http://stackoverflow.com/questions/13006315/how-to-access-data-data-folder-in-android-device)

## adb shell input script

[ainput script on Github](https://github.com/bes/Betup/blob/master/scripts/ainput)

## Android annotation processing

Use Lombok + Android APT

* [Lombok](https://projectlombok.org/setup/android.html)
* [Android-apt](https://bitbucket.org/hvisser/android-apt)

## Android APK debugging

Check for resources inside APK
`~/sdk/android-curr/build-tools/21.1.1/aapt dump resources build/outputs/apk/MyApk.apk | grep raw/`

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

* [Tcpdump binary in android/tcpdump](android/tcpdump)
* [Tcpdump build guide](https://bes.github.io/guide/2016/10/12/android-tcpdump.html)

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
# Unix Common

## Find files while excluding directories

Note from the manual:
```
  Operators
        Operators join together the other items within the expression.
        They include for example -o (meaning logical OR)
        and -a (meaning logical AND).
        Where an operator is missing, -a is assumed.
```

```
gfind . -type d \
  \( \
    -path ./node_modules \
    -o -path ./.git \
  \) \
  -prune \
  -o -type f -name "*.ts*" -print | gxargs wc -l
```

So logically this means

- Find from `.` all -type d
  - And from that list if the path is
    - node_modiles
    - OR .git
  - Then -prune the list of those directories
- After pruning

## Make sudo password entry visible character-by-character

```
sudo visudo

# Find this line
Defaults        env_reset

# Change to this line
Defaults        env_reset,pwfeedback

# Now timeout your sudo session and test it
sudo -k

# Should prompt your password again
sudo visudo
```

* [Source](http://www.howtogeek.com/194010/how-to-make-password-asterisks-visible-in-the-terminal-window-in-linux/)
* [Source](http://lifehacker.com/make-password-asterisks-visible-in-your-linux-terminal-1183533223)

## Make a random string

```
RANDOM_STRING=$(env LC_CTYPE=C tr -dc 'a-zA-Z0-9' < /dev/urandom | fold -w 64 | head -n 1)
```

## Make a random big file

### Linux

Poll status of running dd, (status will be printed in the terminal running dd).

```
sudo dd if=/dev/urandom of=./bigfile bs=1M count=458000

#check status
sudo kill -USR1 `pidof dd`
```

### macOS


```
sudo dd if=/dev/urandom of=./bigfile bs=1m count=458000
#check status
sudo kill -INFO `pidof dd`
```

## Loop over a number of files and execute a command for each one

```
for f in $(find -name "*.apk"); do adb install -r $f; done
```

## Remove CRLF at the end of lines in a file

Use dos2unix command in ubuntu

## List all open files and the program that is keeping the file open

```
lsof
```

Or on macos you can also use

```
sudo fs_usage
```

## Find my external ip using the command line

```
dig +short myip.opendns.com @resolver1.opendns.com
```

* [Source](http://askubuntu.com/a/145017/221354)
* [Also available on Betup](https://github.com/bes/Betup/blob/master/scripts/myip)

## Peak memory usage of a process run

Use the [memusg](https://gist.github.com/netj/526585) script, as explained
[here](http://stackoverflow.com/a/3491573/816017).





[](================================================================================================================)
# Jekyll

```
# While developing
jekyll serve
# Before launching
jekyll build
```





[](================================================================================================================)
# Environments (rbenv, jenv...)

* [rbenv](https://github.com/rbenv/rbenv)
* [jenv](http://www.jenv.be/)





[](================================================================================================================)
# iOS / XCode

## Investigate crash without proper stack with attached debugger

First try to find the memory address of the instance you want to inspect. That can be done with e.g. the memory graph
in XCode.

Then, in the LLDB prompt:

```
(lldb) expr -l Swift -- let $my = unsafeBitCast(0x98f63c31, to: MyClass.self)
(lldb) expr -l Swift -- print($my.thing)
```

## Get list of running and shut down emulators

```
xcrun simctl list devices

#or

xcrun simctl list devices | grep Booted
```

## XCode Cleanup

* Archives - Delete from Organizer
* Simulators - Delete from XCode then `xcrun simctl delete unavailable`
* Device debug symbols - Delete unwanted from `~/Library/Developer/Xcode/iOS DeviceSupport/`

## Directory of a simulator

```
~/Library/Developer/CoreSimulator/Devices/<DEVICE_ID_FROM_SIMCTL>/
```

## Concurrency
[GCD tutorial @ raywenderlich](http://www.raywenderlich.com/79149/grand-central-dispatch-tutorial-swift-part-1)

* Queue types
 * Custom Serial Queue
 * Global Concurrent Queue
 * Custom Concurrent Queue

* Concurrent queues
 * QOS_CLASS_USER_INTERACTIVE: UI updates, small workloads
 * QOS_CLASS_USER_INITIATED: Waiting for immediate result, required for continued user interaction
 * QOS_CLASS_UTILITY: Long running tasks with progress
 * QOS_CLASS_BACKGROUND: User is unaware

* Methods
 * dispatch_get_main_queue
 * dispatch_get_global_queue
 * dispatch_queue_create: instantiate with reverse DNS style name
  * DISPATCH_QUEUE_CONCURRENT
  * DISPATCH_QUEUE_SERIAL (or) nil / NULL / 0: Serial queue
  * dispatch_queue_attr_make_with_qos_class: Usage = private let serialQueue = dispatch_queue_create("my.reverse.dns", dispatch_queue_attr_make_with_qos_class(DISPATCH_QUEUE_SERIAL, QOS_CLASS_BACKGROUND, 0))
 * dispatch_after
 * dispatch_sync:
 * dispatch_async
 * dispatch_barrier_async

* Dispatch barrier (dispatch_barrier_async) advice
 * Custom Serial Queue: Don't use dispatch barrier, doesn't make sense
 * Global Concurrent Queue: Be careful, can starve other global tasks
 * Custom Concurrent Queue: Do use






[](================================================================================================================)
# Testing network conditions using WiFi
* [How to test your software under bad network conditions](NetworkConditionsTesting.md)







[](================================================================================================================)
# Python

[Dive Into Python 3](http://www.diveintopython3.net/)



## Miniconda

### Install

```
brew cask install miniconda
```

### Aliases

I have put a custom conda plugin in .oh-my-zsh

### Other stuff

```bash
#Check which sandboxes you already have
conda info -e

#Create a new sandbox
conda create -n my_sandbox python=3.6

#Use the sandbox in this terminal
source activate my_sandbox

#iPython is probably useful
conda install ipython

#When you feel done use
source deactivate
```

## Use iPython

```python
$ ipython
from webmonkey import monkey
mon = monkey.SW()
mon.driver...
```





[](================================================================================================================)
# Javascript

## Node

### Stringify

JSON.stringify is great but sometimes complex node objects have recursive pointers.

Use this instead:

```
const util = require('util')

console.log(util.inspect(myObject, {showHidden: false, depth: null}))
```

Or just use `console.log` or `console.dir` which use it implicitly.

### NVM

* [NVM on Github](https://github.com/creationix/nvm)

Example: Node 7.10 and NPM 5.0.1

```
nvm install 7.10
nvm use 7.10
npm install -g npm@5.0.1
```


### packages

* [ncu - npm-check-updates](https://www.npmjs.com/package/npm-check-updates)


## Prepare a canvas for HiDPI use

```
pixelRatio(canvas) {
  var ctx = canvas.getContext("2d"),
    dpr = window.devicePixelRatio || 1,
    bsr = ctx.webkitBackingStorePixelRatio ||
      ctx.mozBackingStorePixelRatio ||
      ctx.msBackingStorePixelRatio ||
      ctx.oBackingStorePixelRatio ||
      ctx.backingStorePixelRatio || 1;

  return dpr / bsr;
}

prepareCanvas(canvas, width, height) {
  var pixelRatio = this.pixelRatio(canvas);
  canvas.width = width * pixelRatio;
  canvas.height = height * pixelRatio;
  canvas.style.width = width + "px";
  canvas.style.height = height + "px";
  return pixelRatio;
}
```


[](================================================================================================================)
# Virtual machine using Vagrant & VirtualBox

[Vagrant](https://www.vagrantup.com/) is an easy way to set up virtual machines. Don't forget to install [VirtualBox](https://www.virtualbox.org/).

## Vagrantfile example

Place [this file](vagrant/Vagrantfile) in `~/vagrant/Vagrantfile`

## Vagrant up

To start the machine in go to the directory with the Vagrantfile and do

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

#Upgrade e.g. ubuntu
vagrant box update
```






[](================================================================================================================)
# nmap

## List all IP addresses of a certain subnet
```
nmap -sn 192.168.1.0/24
```





[](================================================================================================================)
# macOS
## Settings

Keyboard repeat rate - sign out required?
```
defaults write -g KeyRepeat -float 1.8
defaults write -g InitialKeyRepeat -int 15
```

Dock animation speed
```
defaults write com.apple.dock autohide-time-modifier -int 0;killall Dock
```

Show hidden files
```
defaults write com.apple.finder AppleShowAllFiles YES;killall Finder
```

Key repeat not working in some apps, reboot needed:
```
defaults write -g ApplePressAndHoldEnabled -bool false
```

Set the screenshot directory
```
defaults write com.apple.screencapture location /Users/myuser/some/path
```

Turn off charging hardware chime
```
defaults write com.apple.PowerChime ChimeOnNoHardware -bool true
killall PowerChime
```

## Kernel modules extensions (.kext)

Kernel modules can be located in these locations
* User kexts `/Library/Extensions`
* System kexts `/System/Library/Extensions`

### List loaded modules

```
kextstat
```

### Uninstall a .kext

```
sudo kextunload /Library/Extensions/SomeKernelExtension.kext
sudo rm -rf /Library/Extensions/SomeKernelExtension.kext
```

Also check if there are things hanging around in `Receipts`

```
# Remove any Receipts that belong to a .kext
lsa -lsa /Library/Receipts/
```

## Multiple disk dropbox sync

Follow this [guide](http://lifehacker.com/5971204/run-multiple-dropbox-accounts-on-one-computer) on Lifehacker. Or from the [original source](http://theterran.com/blog/2012/6/14/use-two-dropbox-accounts-on-one-computer.html#fn:3).

But use the modified version below to avoid the Automator Cog, which shows up unless you redirect the log to `/dev/null`.

Basically:

* Create a folder somewhere, e.g. in home folder called `~/.dropbox2`
* In Automator
    * File > New > Application option
    * Drag the "Run Shell Script" action into the main window
    * Paste this `HOME=$HOME/.dropbox2 /Applications/Dropbox.app/Contents/MacOS/Dropbox &>/dev/null &`
    * When the disk is attached, run the program - Before the disk is detached, close Dropbox2 (exit the second dropbox instance).

## Check what is preventing your computer from going to sleep

```
pmset -g assertions
```

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


### Copy public key
```
pbcopy < ~/.ssh/id_rsa.pub
```

## List of OSX Apps
### Essential
* [Homebrew](http://brew.sh/) - brew for short.
 * [Homebrew FAQ](https://github.com/Homebrew/homebrew/blob/master/share/doc/homebrew/FAQ.md)
* [iTerm2 (version 3)](https://www.iterm2.com/version3.html)
* [Zsh / brew](http://rick.cogley.info/post/use-homebrew-zsh-instead-of-the-osx-default/)
 * [Oh-My-Zsh GitHub](https://github.com/robbyrussell/oh-my-zsh), [http://ohmyz.sh/](http://ohmyz.sh/), [Themes](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)
* Git - install using brew (see further instructions below)
 * [Kdiff3](http://kdiff3.sourceforge.net/) For git-mergetool ([Set-up](http://stackoverflow.com/questions/9776434/git-mergetool-config-on-mac-osx))
   * git config --global merge.tool kdiff3
   http://stackoverflow.com/questions/9776434/git-mergetool-config-on-mac-osx
 * [Retina gitk](http://superuser.com/questions/620824/is-it-possible-to-have-git-gui-gitk-look-good-on-a-retina-macbook-pro)
  * [Retinizer](http://retinizer.mikelpr.com/) - Make non-retina apps into retina apps (e.g. Gitk)
   * brew cask install retinizer
   * open /System/Library/Frameworks/Tk.framework/Versions/Current/Resources/
   * Drag Wish and drop into Retinizer
* [KeeWeb](https://keeweb.info/) (Native Electron-based KeePass2 app for MacOS)
* [Hyperswitch](https://bahoom.com/hyperswitch) Command+Tab replacement
* [Smooth Mouse](http://smoothmouse.com/) for turning off mouse acceleration - Does not work for Sierra :/
* [Karabiner](https://pqrs.org/osx/karabiner/) for increasing repeat rate and decreasing repeat wait timer
* [Atom](https://atom.io/) Competent text editor by Github
* [Sublime Text](https://www.sublimetext.com/)
* [SizeUp](http://www.irradiatedsoftware.com/sizeup/) by IrradiatedSoftware (maximize / center / snapback windows)
* [iStat Menus](https://bjango.com/mac/istatmenus/) Some useful status bar menus (CPU, Network, Calendar)
* XCode
* [RVM](https://rvm.io/rvm/install) Run multiple Ruby versions simultaneously.
* [Finderpath](https://bahoom.com/finderpath/) (Command+G path in finder)

### Optional
* TLDR man pages: `brew tap tldr-pages/tldr && brew install tldr`
* [XnViewMP](http://www.xnview.com/) image viewer / editor
* [CyberDuck FTP Client](https://cyberduck.io/)
* Soundflower advanced sound input/output adjustment [This fork seems best right now](https://github
.com/mLupine/SoundflowerBed/releases)
* [VeraCrypt](https://veracrypt.codeplex.com/) If you need that sort of thing
 * [OSX Fuse](https://osxfuse.github.io/) Needed by VeraCrypt
* [GPGTools](https://gpgtools.org/)
* [Ukelele Keyboard Layout Editor](http://scripts.sil.org/ukelele)
* [qBittorrent](http://www.qbittorrent.org/)
* Chat apps (Gtalk)
 * Adium (Free, uglier than Flamingo but has better notifications)
* [Livestreamer](https://github.com/chrippa/livestreamer) (pip install livestreamer // livestreamer <Twitch URL>)
* FFMPEG `brew install ffmpeg --with-openssl --with-fdk-aac --with-ffplay --with-freetype --with-libass --with-libquvi --with-libvorbis --with-libvpx --with-opus --with-x265`
* [Tunnelblick OpenVPN Client](https://tunnelblick.net/)

[Lifehacker essential mac list](http://lifehacker.com/lifehacker-pack-for-mac-our-list-of-the-essential-mac-635303836)

## SDKMAN!

SDK Manager for Java based languages [https://sdkman.io/](https://sdkman.io/).

## Brew / Homebrew

### Create a brew bundle

First install brew bundle by simply writing `brew bundle` in the terminal, which is also the command for
installing everything in the bundle file in the current directory.

Create a [Homebrew bundle](https://github.com/Homebrew/homebrew-bundle) in the form of a `Brewfile`,
which declares all taps and packages that you have installed.

Create  a `Brewfile` from scratch in the current directory:

```
brew bundle dump --file ./Brewfile --force
```

To uninstall all Homebrew formulae not listed in Brewfile:

```
brew bundle cleanup
```

### List packages
```
brew list
```

### Fetch new package info
```
brew update
```

### List outdated packages
```
brew outdated
```

### Upgrade outdated package
```
brew upgrade <pkg>
```

### Upgrade all packages
```
brew upgrade
```

### Cleanup / Remove unused versions

This command is very useful because a lot of space can be wasted on unused versions

```
brew cleanup
```



### Essential brew packages (with explanations)

Install with brew install <name>

* awscli
* bmon - (network monitoring)
* curl
* [findutils](https://www.gnu.org/software/findutils/) - (GNU findutils gfind, gxargs, ...)
* gradle
* [grep] - (ggrep)
* groovy - (Groovy programming language)
* [moreutils](https://joeyh.name/code/moreutils/) (contains ts for prepending datetime to terminal lines)
* [nmap](https://nmap.org/) - (Network mapper)
* node - (NodeJS)
* openssl
* [pidof](https://linux.die.net/man/8/pidof) - (find PIDs of named program)
* swiftlint - (XCode swift linting)
* tree
* unrar
* wget
* zsh
* OpenJDK
    * `brew tap AdoptOpenJDK/openjdk` then `brew cask install adoptopenjdk11`
    * [AdoptOpenJDK](https://github.com/AdoptOpenJDK/homebrew-openjdk)

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

According to README.md in `~/.oh-my-zsh/plugins/git-extras`:

On OS X with Homebrew, you need to install `git` with `brew install git --without-completions`. Otherwise, `git`'s `_git` will take precedence, and you won't see the completions for `git-extras` commands.

Then install git-extras itself via `brew install git-extras`

## Monitor network / bandwidth with bmon

```
brew install bmon
```

## Stay sane

* [Re-map Command/Option key on Windows USB keyboard) (Instead of losing your mind)](http://superuser.com/questions/80976/how-to-re-map-command-and-option-keys-on-mac-os-x-with-a-pc-keyboard)
* [Word/Line deletion in iTerm2](https://coderwall.com/p/ds2dha/word-line-deletion-and-navigation-shortcuts-in-iterm2)

## Workarounds

### IntelliJ can't find git executable for use in exec()

If you installed git from git-scm.org according to the instructions, you need to symlink /usr/bin/git to /usr/local/git/bin/git otherwise intellij can't find git in exec().

## Potentially useful

* [JEnv](http://www.jenv.be/) Set Java Home, locally for a given path if you want.

## Launch an app from terminal

Example with Atlassian SourceTree

```
open -a SourceTree <path/to/repository>
```

## Launch an OSX app from terminal with logs from that app

Example: QuickTime Player

```
cd /Applications/QuickTime Player.app/Contents/MacOS%
./QuickTime\ Player
```

## File system tracing OSX

Same as Unix/Linux

```
lsof
```

## Erase SSD / Hard drive

* Turn on computer
* When the mac sound plays press `⌘ + R`
* Enter disk utility and mount the disk (if it is encrypted it is not auto mounted)
* Exit disk utility, enter Terminal via menu bar
* In terminal write diskutil `secureErase freespace 4 /Volumes/Macintosh\ HD`
 * If your disk is named something else you need to adapt the command

[Source](https://www.backblaze.com/blog/securely-erase-mac-ssd/)

## Somewhat updated list of brew packages (brew list)

`% brew list`

```
autoconf    gawk      libffi      mtr     socat
automake    gdbm      libgpg-error    nmap      sqlite
awscli      gettext     libksba     node      stress
bash      git     libogg      openssl     swiftlint
bmon      glib      libpng      openvpn     tcl-tk
cairo     gmp     libquvi     opus      texi2html
cmake     gnu-sed     libtool     pcre      tldr
confuse     gobject-introspection libvo-aacenc    pidof     tree
curl      gradle      libvorbis   pixman      unrar
dos2unix    grep      libyaml     pkg-config    wget
faac      groovy      libzip      protobuf    x264
fdk-aac     harfbuzz    lua     protobuf-swift    xvid
findutils   icdiff      lzo     python3     xz
fontconfig    icu4c     maven     qt      yasm
freetype    lame      mpfr      readline    zeromq
fribidi     libass      mplayer     sdl     zsh
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

## Sudoers

This command edits the sudoers file:

```
sudo visudo
```

Here is an [explanation on askubuntu](http://askubuntu.com/a/443071).

Another way of doing it is adding only your own user as a sudoer:

```
myusername     ALL=(ALL:ALL) NOPASSWD:ALL
```

## Java alternatives

Used to select what java versions are used, on Ubuntu Linux (probably other dists as well)

```
$ sudo update-alternatives --config java
$ sudo update-alternatives --config javac
$ sudo update-alternatives --config jar
$ update-java-alternatives -l
```

## Gnome 3

Gnome HiDPI info on [Archlinux wiki](https://wiki.archlinux.org/index.php/HiDPI#GNOME).

Basically you first scale up to 2x2 (X only supports integer scaling, Wayland probably will handle floats):

```
gsettings set org.gnome.desktop.interface scaling-factor 2
# There is a UI setting for this in Gnome, but I didn't feel it went "all the way":
Gnome Tweak Tool -> Windows -> HiDPI -> Window scaling = 2
```

Now you can "Zoom Out":

```
# Fetch display name
xrandr | grep -v disconnected | grep connected | cut -d' ' -f1
# Zoom out
xrandr --output <DISPLAY NAME FROM PREVIOUS COMMAND> --scale 1.2x1.2
```

If you are having problems with e.g. mouse: See archlinux wiki above.

## OpenVPN
[Example configuration files here](OpenVPN/)

### Configure the server
Configuration in `/etc/openvpn/`. Client config in .ovpn files.

```
iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o <DEVICE e.g. eth0 but probably something else> -j MASQUERADE
```

And clear the rule when done

```
sudo iptables -t nat -F
```

### Using the server:

Update local `name.ovpn` file with the current correct external IP of the VPN server.

"server" in the command is the name of the configuration file

```
sudo systemctl start openvpn@server
```

Restart

```
sudo systemctl restart openvpn@server
```

### Configure ASUS router

* [VPN Config](https://support.hidemyass.com/hc/en-us/articles/204449557-Asus-VPN-Client-Setup-Original-firmware-)


* [The OpenVPN Wiki](https://community.openvpn.net/openvpn/wiki/HOWTO)
* [Here is a guide](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-14-04) from Digitalocean.
* [And here is another guide from the Ubuntu 14.04 help section](https://help.ubuntu.com/14.04/serverguide/openvpn.html)
* [How to set up the ovpn file](http://serverfault.com/questions/483941/generate-an-openvpn-profile-for-client-user-to-import)
* [Connect to a VPN using the command line and an ovpn file](http://superuser.com/questions/561816/using-openvpn-from-mac-osx-terminal-cannot-load-tun-tap)

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






[](================================================================================================================)

# Quotes

## Collection of development related lol-quotes

* ["GCD and dispatch_queue_t are still a masterpiece and the API works great in Swift"](https://www.mikeash.com/pyblog/friday-qa-2015-02-06-locks-thread-safety-and-swift.html)
* ["Do you ever have the feeling that computer architectures come and go, but CA:AQA is forever?"](https://www.goodreads.com/review/show/1253988725?book_show_action=true)





[](================================================================================================================)

# Unread blog post pipeline

* [Blue team rust](https://tiemoko.com/blog/blue-team-rust/)
* [Code and bitters](https://codeandbitters.com/)
* [Lifetime variance](https://github.com/sunshowers/lifetime-variance-example)
* [AWK in 20 minutes](https://ferd.ca/awk-in-20-minutes.html)
* [Futures Explained](https://cfsamson.github.io/books-futures-explained/introduction.html)
* [ELF format blog series by fasterthanlime](https://fasterthanli.me/series/making-our-own-executable-packer/part-13)

# Incomplete list of nice blog posts and such

* [Cryptopals: Learn stuff about crypto by doing excercises](https://cryptopals.com/)
* [Google Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course)
* [Kill Math: Intuition guided math solving](http://worrydream.com/KillMath/)
* [Azeria ARM (exploitation) tutorials](https://azeria-labs.com/writing-arm-assembly-part-1/)
* [AI Matrix Calculus](http://explained.ai/matrix-calculus/index.html)