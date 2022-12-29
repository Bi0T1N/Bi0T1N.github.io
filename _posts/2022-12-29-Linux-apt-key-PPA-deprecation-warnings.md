---
layout: default
title:  "Fix apt-key deprecation warnings for PPA packages - the easy way"
categories: [linux, mint, ubuntu, jammy, apt-key, deprecation, ppa, launchpad]
---

# {{ page.title }}

## Introduction

In Ubuntu 22.04 ("Jammy Jellyfish") and Debian 11 ("bullseye") the use of `apt-key` is deprecated. These versions are also the last ones that are shipped with this key management utility. Therefore you will get deprecation warnings on `apt update` commands if you've previously installed keys with this method. Older keys installed automatically via the Personal Package Archives (PPA) on Ubuntu also cause this warning.

## Warning message

When you run `apt update` you see following warning about the deprecation of the legacy trusted.gpg keyring:
```bash
W: http://ppa.launchpad.net/bit-team/stable/ubuntu/dists/jammy/InRelease: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.
```

## Solution

To get rid of the deprecation warning of programs installed via the PPA on an older distribution like Ubuntu 20.04 ("Focal Fossa") the simplest solution is to remove the two entries via the Linux Mint application (`Control Center -> Software Sources -> PPAs`) and then re-add the PPA repository by executing the command to add a new PPA package.  
Thus for the ["Back in Time" application](https://launchpad.net/~bit-team/+archive/ubuntu/stable) used as example here, remove the old entries first.

![Old PPA entry for "Back in Time" from previous installation on Ubuntu focal](/images/ppa-existing-old-entry.png)

Afterwards run `sudo add-apt-repository ppa:bit-team/stable` which will result in following output
```bash
You are about to add the following PPA:
 This repository contains stable releases for Back In Time.
 More info: https://launchpad.net/~bit-team/+archive/ubuntu/stable
Press Enter to continue or Ctrl+C to cancel

gpg: keybox '/etc/apt/keyrings/589EEDCD16567B0E6D23C3144B6071B7D6FDC9D0.keyring' created
gpg: key 4B6071B7D6FDC9D0: public key "Launchpad Stable repository" imported
gpg: Total number processed: 1
gpg:               imported: 1
```
Now it uses the recommended approach of placing the key somewhere in the filesystem (`/etc/apt/keyrings/`) and referencing it within the source list via the `Signed-By` option.

![New PPA entry for "Back in Time" with the signed-by syntax](/images/ppa-entry-with-new-syntax.png)

Thus after you run `sudo apt update` you will no longer see a deprecation warning.

### Additional notes for packages that are not yet available for your distribution
If you try to fix a program which doesn't provide a [PPA package](https://launchpad.net/~lyx-devel/+archive/ubuntu/release) for your distribution yet, you'll get following error after running the `add-apt-repository` command:
```bash
Cannot add PPA: ''This PPA does not support jammy''.
```
As we can see it on the Launchpad site LyX doesn't support Ubuntu 22.04 yet and thus no `jammy` package exists.
!["LyX" repository still without Jammy package](/images/launchpad-lyx-missing-package-jammy.png)
If we look at the Back in Time repository, we will see that it already lists the package for `jammy` and so this easy approach works well.
!["Back in Time" repository with Jammy package](/images/launchpad-backintime-packages.png)

## Sources

[Personal Package Archives : Ubuntu](https://launchpad.net/ubuntu/+ppas)  
[Handling "apt-key is deprecated. Manage keyring files in trusted.gpg.d instead" in Ubuntu Linux](https://itsfoss.com/apt-key-deprecated/)  
[Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead](https://stackoverflow.com/questions/68992799/warning-apt-key-is-deprecated-manage-keyring-files-in-trusted-gpg-d-instead)  
[Ubuntu Manpage: apt-key - Deprecated APT key management utility](https://manpages.ubuntu.com/manpages/jammy/en/man8/apt-key.8.html)  
