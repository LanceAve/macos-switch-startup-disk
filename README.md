# Switch Startup Disk

A tiny macOS Finder Quick Action made using Automator. Its sole purpose is for switching between a bootable macOS volume without digging through System Settings or typing the command manually a billion times. 

## What's it for

It essentially just wraps Apple’s `bless` command in a Finder-friendly Quick Action (similar in spirit to the [MountEFI tool](https://github.com/corpnewt/MountEFI) made by [CorpNewt](https://github.com/corpnewt).)

The important bit lies here for the most part:
```zsh
/usr/sbin/bless --mount "/Volumes/Some Boot Drive" --setBoot && /sbin/reboot
```

The `&&` was added intentionally so that the machine only reboots if the `bless` command was successful. 

I learned that the hard way when I was testing the workflow and it rebooted my machine, erasing my entire progress...

To verify if it went through (after it reboots of course,) run: 
```zsh
sudo systemsetup -getstartupdisk
```

then check if it shows the volume you selected. no need to panic immediately. 

If it returns `(null)`, then no need to panic. In my testing, that usually meant there was no explicit startup disk set, which often falls back to the internal macOS install.

## Why

I made this because I wanted a much simpler way to bounce back and forth between an internal macOS install and an external work-focused macOS install <sup><sub>(I like to separate things, okay? Leave me alone.)</sub></sup> 

## Install

Just double-click it and Automator will prompt whether or not you want to add it. 

If it doesn't pop up right away, you can always just run:

```zsh
killall Finder
```
To restart the Finder. 

## Spotlight / Duplicate Apps Issue

I ran into this issue because I basically took a bunch of apps from my main install and plopped it into the drive. Bad idea, apparently, because MacOS will happily index it and you may end up launching an app that's only on that drive. This could spell disaster, because once you unmount, the program could hard crash <sup><sub>(again... learned that the hard way).</sub></sub>

You may want to disable indexing on the other boot volume via the `mdutil` command:

```zsh
sudo mdutil -i off "/Volumes/Some Boot Drive"
sudo mdutil -i off "/Volumes/Some Boot Drive - Data"
```

Or the reverse when booted from the external system:

```zsh
sudo mdutil -i off "/Volumes/Macintosh HD"
sudo mdutil -i off "/Volumes/Macintosh HD - Data"
```

**FYI:** Avoid running that against `/`, unless your intent is to run it on the currently booted volume. 

## Notes

This is mostly intended for bootable macOS volumes on Apple Silicon Macs, though it may or may not work on T2 machines. I don't have access to a T2 machine, so I can't verify if it works. Use at your own risk.

For a much better technical dive, just visit one of Asahi's thorough [documentation](https://asahilinux.org/docs/platform/introduction/) about the boot flow. They're awesome!

This project was inspired by the work of [rxhfcy](https://github.com/rxhfcy) with their [Asahi Restart Helper tool](https://github.com/rxhfcy/Asahi-Restart-Helper--macOS-version) Theirs is much fancier, but I wanted an easier and less intrusive version <sup><sub>(that didn't require me to burn time on SwiftUI)</sub></sub>

## Credits

- [Apple](https://www.apple.com) for macOS, Finder, Automator, AppleScript and `bless` command
- [Asahi Linux](https://asahilinux.org/) for making me even realise that `bless` is a thing.
- [CorpNewt](https://github.com/corpnewt) for the inspiration to make a similar tool. <sup><sub><sup><sub>(R.I.P Hackintosh. Gone but not forgotten...)</sub></sub>
- [rxhfcy](https://github.com/rxhfcy) for making me realise it's feasible and not just some pipe dream.

## License
This project is licensed under the MIT License. See [**LICENSE**](LICENSE.md) for more details.
