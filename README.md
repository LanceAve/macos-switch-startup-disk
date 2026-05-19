# Switch Startup Disk

A tiny macOS Finder Quick Action that was made using the Automator app on MacOS. Its sole purpose is to allow you to switch between a bootable macOS volume without having to dig through System Settings over and over again, or having to type the command manually a billion times. 

It essentially just wraps Apple’s `bless` command in a Finder-friendly Quick Action (similar in spirit to the [MountEFI tool](https://github.com/corpnewt/MountEFI) made by [CorpNewt](https://github.com/corpnewt).)

### How it works

Literally just how you type it out on the terminal using:
```zsh
/usr/sbin/bless --mount "/Volumes/Some Boot Drive" --setBoot && /sbin/reboot
```

Except it explictly specifies the path of bless and reboot commands. I added the `&&` on purpose so that if, for whatever reason, the `bless` command failed, the machine won't just reboot anyhow.

> [!CAUTION]
> Be sure to **SAVE YOUR PROGRESS BEFORE YOU RUN THIS**. Unlike clicking reboot via the menu bar, this doesn't prompt you to save your work and the such. Which means that anything you did can and might be discarded if you don't save it. I'm not responsible if you lost your 40 page thesis mid-way or whatever. You've been warned.


Also, this intentionally avoids using sudo so you don't have to give sudoless permission to something like this (mostly a security precaution on my end.)

I learned that the hard way when I was initially testing the workflow. It ended up rebooting my machine mid-development which erased all of my progress...


> [!TIP]
> To verify if it went through (after it reboots of course) just run this command:
> 
> ```zsh
> sudo systemsetup -getstartupdisk
> ```
> 
> to check if it shows you the volume that you selected.
> 
> If it returns `(null)`, no need to panic. In the testing I did, that usually just meant that there was no explicit startup disk that was set, which often times ended up just falling back to the internal macOS install. (it didn't boot me to the recovery environment from my attempts, though your mileage may vary.)

### Why?

I wanted to make this to create a fully seperate environment for work stuff and personal stuff on my little MacBook.  <sup><sub>(I like to separate things, okay? Leave me alone.)</sub></sup> 

### Install

Just double-click it and Automator will prompt whether or not you want to add it. 

If it doesn't pop up right away, you can always just run:

```zsh
killall Finder
```
To restart the Finder. 

> [!NOTE]
> ### Spotlight / Duplicate Apps Issue
> 
> I ran into this issue because I basically took a bunch of apps from my main install and plopped it into the drive. Bad idea, apparently, because MacOS will happily index it and you may end up launching an app that's only on that drive. This could spell disaster, because once you unmount, the program could hard crash <sup><sub>(again... learned that the hard way).</sub></sub>
>
> You may want to disable indexing on the other boot volume via the `mdutil` command:
>
> ```zsh
> sudo mdutil -i off "/Volumes/Some Boot Drive"
> sudo mdutil -i off "/Volumes/Some Boot Drive - Data"
> ```
> 
> Or the reverse when booted from the external system:
> 
> ```zsh
> sudo mdutil -i off "/Volumes/Macintosh HD"
> sudo mdutil -i off "/Volumes/Macintosh HD - Data"
> ```

> [!TIP]
> **FYI:** Avoid running that against `/`, unless you're intentionally running it on the currently booted volume. 

### Notes

This is mostly intended for bootable macOS volumes on Apple Silicon Macs, though it may or may not work on T2 machines. I don't have access to a T2 machine, so I can't verify if it works so ¯\_(ツ)_/¯

For a much better technical dive, just visit one of Asahi's thorough [documentation](https://asahilinux.org/docs/platform/introduction/) about the boot flow. They're awesome!

This project was inspired by the work of [rxhfcy](https://github.com/rxhfcy) with their [Asahi Restart Helper tool](https://github.com/rxhfcy/Asahi-Restart-Helper--macOS-version) Theirs is much fancier, but I wanted an easier and less intrusive version <sup><sub>(that didn't require me to burn time on SwiftUI)</sub></sub>

### Credits

- [Apple](https://www.apple.com) for macOS, Finder, Automator, AppleScript and the `bless` command
- [Asahi Linux](https://asahilinux.org/) for making me even realise that `bless` is a thing.
- [CorpNewt](https://github.com/corpnewt) for the inspiration to make a similar tool. <sup><sub><sup><sub>(R.I.P Hackintosh. Gone but not forgotten...)</sub></sub>
- [rxhfcy](https://github.com/rxhfcy) for making me realise it's feasible and not just some pipe dream.

## License
This project is licensed under the MIT License. See [**LICENSE**](LICENSE.md) for more details.
