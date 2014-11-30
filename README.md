Ansible playbook for OSX
=======
## Introduction

OSX Playbook assembled from various Ansible Galaxy and GitHub repos. Credits to those who share.

The main focus of this setup is on software development (duh).
See the install.yml for the complete list of packages, and the configuration.yml for what happens with shells and dotfiles.

## This playbook does not overwrite dotfiles

I decided not to overwrite dotfiles, but instead use those that are found. This goes against immutability of the deployment, but we're dealing with an OSX workstation here, and I don't want you to rename files back and forth.
To see what comes out of the box, you have to backup and remove the following files and folders before running the playbook:

 * `~/.common*`
 * `~/.vim*`
 * `~/.zsh*`

 If will attempt to create the following files and symlinks:

 * `~/.common-env`
 * `~/.common-aliases`
 * `~/.vim*`
 * `~/.oh-my-zsh`
 * `~/.zsh*`

 The common files will be shared between bash and zsh.

## Prerequisites

### Sudo

Before you start, make sure you have password-less sudo:

    sudo visudo

If you are asked for a password then you don't have sudo, so go ahead and uncomment the line granting passwordless sudo to the group 'wheel'.

Then add yourself to the 'wheel' group:

    sudo dseditgroup -o edit -a usernametoadd -t user wheel

And if you want to be an admin if you weren't yet (I don't believe this is necessary, but I might be wrong. Hey, I'm admin!):

    sudo dseditgroup -o edit -a usernametoadd -t user admin
    
### Cask app path

To not have brew cask install all apps in `~/Applications`, but in `/Applications` (like the rest of your apps), and to avoid double installs, we need to tell cask about this. So until [this pull request](https://github.com/osxc/homebrew/pull/5) is merged, place this line somewhere in your ~/.bash_profile: `export HOMEBREW_CASK_OPTS="--appdir=/Applications"`
and make sure you source it (`. ~/.bash_profile`) before running the playbook.

### Xcode

Be sure to have the XCode Command-Line tools installed: `xcode-select --install`

## Install

You are now set to go ahead:

1. Manually install Ansible 1.8 with ``git clone https://github.com/ansible/ansible.git ~/src/github.com/ansible/ansible; cd ~/src/github.com/ansible/ansible; git submodule update --init --recursive; sudo make install``
2. While that's happening [Fork this repo](https://github.com/morriz/osx-ansible/fork) and then clone your fork anywhere you want on your machine: `git clone https://github.com:<yourname>/osx-ansible.git osxc; cd osx-ansible`
3. Take a quick look at `configuration.yml` and `installation.yml` customizing to your liking.
4. Start the playbook with `ansible-galaxy install -r requirements.yml && ansible-playbook desktop.yml`

## Credits

 * [rricard](https://github.com/rricard/osxc), who started this fork and published osxc.* tools to install brew and cask packages.
 * [spencergibb](https://github.com/spencergibb/battleschool), for his mac_pkg module to enable manual downloading of dmg and pkg files.
 * [hnakamur](https://github.com/hnakamur), for his atom_packages module.
 * [robbyrussell](https://github.com/robbyrussell/oh-my-zsh), for oh-my-zsh
 * [square](https://github.com/square/maximum-awesome), for maximum-awesome dotfiles.

## Learn More

[Ansible documentation](http://docs.ansible.com/index.html)

If you want to get more documentation or just want to see what osxc can do for you, here's a [repository list on ansible galaxy](https://galaxy.ansible.com/list#/users/6499) where you can find all the publicly available roles for osxc.
