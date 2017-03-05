# Eclim installation via Ansible (draft / unfinished)

This unfinished Ansible role is a WIP, I'm not sure to complete one day. I publish it anyway because it could be a starting point to install [Eclim](http://eclim.org).

## Important note

* For now, this Ansible role depends on variables used in my [VM playbook](https://github.com/webastien/dev-vm)
* There are problems with Xvfb / Eclim daemon / Vim communication (see below)

## What's OK?
* Eclipse installation
* Eclipse PHP plugin installation
* Eclim installation
* Xfvb daemon
* Eclim daemon

## What's not?
Work when loaded manually, but not properly when daemons are auto-loaded at VM boot: They need a few seconds before being usable. Especially the Eclim one which requires that Xvfb be ready. So, Eclim daemon launch the java process and not the VIm link (`~/.eclim/eclim-instances`) + do not detect Xvfb RANDR extension.

I think the better way is to disable the startup auto-load (`sudo update-rc.d eclim remove`) and restart the VM, or removing the last 2 tasks in install-daemon.yml. Then, play with them manually.

A few leads to debug:
* Kill java processes (`sudo pkill java` for example), or `sudo kill -9 PID` (where PID is the correct process ID)
* Stop Eclim daemon service (`sudo service eclim stop`) before to re-start it
* Stop Eclim daemon by running `/opt/eclipse/eclim -command shutdown`
* Check the status of Eclim: `/opt/eclipse/eclim -command ping` (shell) or `:PingEclim` (VIm)

I've put some comments at the end of the taskfile, explaining how to test your installation.

## Why I decide not to go on with?

* Eclim allows a faster autocompletion than my current configuration on big project, but sometimes VIm freezes.
* Eclim doesn't currently support the "jump to definition" feature for PHP (with `:PhpSearchContext`)
* I had some bugs when complete variable names: The begin I typed disappears. I can deal with it, but it's boring.

