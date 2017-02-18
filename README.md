# Eclim installation via Ansible (draft / unfinished)

This unfinished Ansible role is a WIP, I'm not sure to complete one day, but who knows? I publish it anyway for archive, it could serve as a starting point to install [Eclim](http://eclim.org).

**All further notes are written after only some hours experiencing Eclim.**

[Eclim's contributors](https://github.com/ervandew/eclim/graphs/contributors) do a great job, I've seen great videos on YT of users enjoying it, so I'm convinced with a better knowledge of this tool and more time to spend on its settings, it can be a wonderful and powerful source code editor.
---

## Before trying installing Eclim with Ansible

This is a bit heavy: You have to run a program (Eclipse, headless but the core is running) using a VM (java is a virtual machine) + a virtual display server + a runing daemon to communicate with VIm. When you play with it in a Vbox, you're doing a VM-ception :-)

Depending on your connexion and Eclipse server's load, you can drink a coffee while waiting downloads are done. I recommand, in the case of using it with Vagrant/Ansible to install [Vagrant cachier](https://github.com/fgrehm/vagrant-cachier) with cache scope and [generic buckets](http://fgrehm.viewdocs.io/vagrant-cachier/buckets/generic/) setted.

I've only try it with my [development VM](https://github.com/webastien/dev-vm), before eventually install it on my (physical) computers. You'll probably need to adapt the tasksfile if you play it on another machine.

## What's OK?
* Eclipse installation
* Eclipse PHP plugin installation
* Eclim installation (or maybe not)

I've put some comments at the end of the taskfile, explaining how to test your installation.

## What's not?
1. Virtual display (ok, but...)

You can read on [the official documentation](http://eclim.org/install.html#install-headless):
> When starting Xvfb you may receive some errors regarding font paths and possibly dbus and hal, but as long as Xvfb continues to run, you should be able to ignore these errors.

I don't like that and ideally, would like to resolve them ; But I can run Eclim with em, so not a big deal.

2. Eclim daemon

I made it work several times, but sometimes had to launch it twice before it starts. When it starts, VIm Eclim plugin refuses often to communicate with the first time.
Restart VIm solved it... Not a situation I'm OK with when have to use my editor for professional purpose.

**With this role**: The daemon seems to start, but the `~/.eclim/.eclimd_instances` file is not generated, so VIm plugin cannot communicate with Eclipse.

3. Starting the virtual display and the daemon automatically

For now, you have to execute those commands manually: (each time the machine start and as the user who will launch VIm)

* `Xvfb :1 -screen 0 1024x768x24 +extension RANDR &` to create the virtual display
* `DISPLAY=:1 /opt/eclipse/eclimd -b -Xmx256M` to start the Eclim daemon

## Why I decide not to go on with?

* Eclim allows a faster autocompletion than my current configuration on big project, but sometimes VIm freezes.

I had to hit CTRL-C to cancel the action Eclim plugin was performing to be able moving again the cursor or close the editor.

* The completion proposals are not always well contextualized

Seems having a fallback to "all methods find in all classes" (or something like that)... So, if auto-complete gives me wrong list of classes' methods I have no benefits to use it, because I will have to open the associated source code to verify by myself. Not a gain of time.

FYI, I tried [Sublime Text](https://www.sublimetext.com) and got similar experience.

