%title: ★  command line magic ★
%author: @orrc
%date: 2018-02-28

\--------------------------------------------------------

---

> "Something about various relatively unknown
> features and command line tools that are
> useful for Android development"
^

\— No Android Studio
\— Only using the terminal
^
\— (mostly)

---

-> # Tools of the talk <-
^

# mdp — markdown -> presentation
https://github.com/visit1985/mdp
^

# iTerm2 — terminal emulator
^

# tmux — terminal multiplexer
^

# bash — just some shell

---

-> # For the  command line users <-

-> # Homebrew <-
-> Package manager <-
-> https://brew.sh/
^

## Apple stupidity
$ *git status*

Agreeing to the Xcode/iOS license requires
admin privileges, please re-run as root via sudo.
^

$ *cowsay wtf*
\ _______
< wtf >
\ -----
\       \\   ^__^
        \\  (oo)\_______
           (__)\       )\\/\\
               ||----w |
               ||     ||
^

## Install newer, sane versions
$ *brew install bash*
$ *brew install git*
^

## No more mounting .dmg files
$ *brew tap caskroom/cask*
$ *brew cask install intellij-idea-ce*
$ *brew cask install java*
$ *brew cask install google-chrome*

---


-> # tmux <-
-> https://tmux.github.io/ <-

## Like `screen`, but better
^

\- Multiple panes
\- Resize, zoom
^

\- Multiple windows
^

\- Multiple sessions
\- Detach / reattach

---

-> # More shell must-haves <-

## autojump
"A *cd* command that learns"
https://github.com/wting/autojump

$ *j sdk*
/Users/chris/android/sdk

$ *j dow*
/Users/chris/Downloads

(plus tab-completion)
^


## highlight
Pretty source code colours in your shell
$ *echo 'export LESSOPEN="| highlight %s --out-format xterm256 --quiet"' >> ~/.bashrc*

---

-> # Yet more shell must-haves <-

## fzf
"A command-line fuzzy finder"
https://github.com/junegunn/fzf


> "Maybe the greatest piece of software ever written"
^
>  — me
^


Amazing shell integration
^


More about fzf later… 🤔

---

-> # Isn't this an Android meetup? <-
^

-> # Debugging <-
^

# pidcat
Filter by app ID, group by tag, colourful
$ *pidcat -w18 -t OkHttp com.example*
^


# adb logcat
Same old command, fancy new arguments
^


Filtering by regex, with nice output:
$ *adb logcat -v brief -e '--> GET|<-- 40[0-9]' | pidcat*
OkHttp  D  --> GET https://api/users/foo http/1.1
        D  <-- 404  https://api/users/foo (281ms)
        D  --> GET https://api/users/.. http/1.1
        D  <-- 400  https://api/users/.. (94ms)
^


Finding crashiest apps:
$ *adb logcat -S -b crash*
Chattiest UIDs in crash log buffer:
UID    PACKAGE                     BYTES
10268  de.materna.bbk.mobile.app    2324

---

-> # Debugging <-

# Getting lifecycle events
$ *adb logcat -v brief -b events \\*
    *| grep --line-buffered am_ | pidcat*
^


# Accessing the filesystem for an app
$ *adb shell run-as <app-id>*
^


# Debugging notifications
\- *adb shell dumpsys notification*
\- Filter for *notification_enqueue* in *events* log
^
\- Use the magical hidden Notification Log UI…

---

-> # Poking at app internals <-

# pm list packages
Fetch the filename of any APK on the device
$ *adb shell pm list packages -f*
^
(recently moved to *cmd package list packages*)
^

$ *adb pull /data/app/some.app.apk*
^


# aapt dump
Getting information from an APK
$ *aapt dump badging some.apk | head
package: name='com.Slack' versionCode='20009074'
 versionName='2.52.0'
^


# fzf
Machine-readable output from *adb* + *aapt* + *fzf*…

\- <insert super-fancy demo of *fzf*>

---

-> # More poking at app internals <-

# apktool
Taking apps apart (and putting them back together)
$ *apktool d some-app.apk*
$ *cd some-app*
$ *# fiddle with some stuff*
$ *apktool b .*
$ *# sign APK and install*
^


# dumpsys activity top
Similar: hierarchyviewer or uiautomaterviewer
Shows the view hierarchy with the actual class names…
$ *adb shell dumpsys activity top*
^

---

-> # Poking at Android internals <-

# dumpsys

# service
Debug Binder interface for system services
$ *adb shell service list*
$ *adb shell service call audio 31 s16 hello*

---

-> # Working with devices & emulators <-

# Text input
Avoid having to type in long text…
$ *adb shell input text Hey*
$ *adb shell input text "'Shells within shells'"*
$ *adb shell input keyevent BACK*
^


# Magical telnet interface
$ *adb devices*
List of devices attached
emulator-5554    device

$ *telnet localhost 5554*
Network state, SMS, phone calls, GPS…

$ *echo 'sms send +49123 hello' | nc localhost 5554*

---

-> # Yes, he's still talking about *fzf* <-

# More use cases
\- Switching between *recent* git branches
\- Installing APK(s) on device(s)
\- Anything you can think of…

---

-> # 👋 <-

\- Get *apktool*, *autojump*, *pidcat*, *tmux*…
\- Automate some tasks you do often with *fzf*
\- Explore the various *adb shell* commands

---
