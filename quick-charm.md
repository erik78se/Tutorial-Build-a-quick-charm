# Quick-charm

# Tutorial: A quick charm

Difficulty: Beginner
Author: [Erik Lönroth]

## What you will learn
This tutorial is a good start to learn about the concept of "juju hooks".

You will:

* Learn how to retrieve a charm from charmstore.
* Learn how to use juju [hooks] in charms.
* Get an overview about the juju event-loop.
* Learn how to use the juju command: "juju run" to execute remote commands.
* Learn how to use 'juju debug-log' for debuging your charms.

Lets get started.
# Preparations
1. You need an internet connection without explicit proxy.
1. You need a juju controller to deploy charms through. Here is how you can get that running on your local machine: [juju-lxd]
1. You need a basic charm developer environment. If you have completed [part 1] "Setup up a basic workbench", then your'e all set.

# The "tiny-python" charm
I have prepared a charm already for you. Its called "[tiny-python]". We will modify tiny-python during this tutorial to learn about juju:s "hooks".

Your first task is to download/pull the tiny-python charm. Go ahead and run:

```bash
charm pull cs:~erik-lonroth/tiny-python
```
This command dowloads the latest version of [tiny-python] from the charmstore to your local filesystem. Lets look at what we got.
```
$ tree tiny-python
tiny-python
├── config.yaml
├── copyright
├── hooks
│   ├── config-changed
│   ├── install
│   ├── leader-elected
│   ├── leader-settings-changed
│   ├── setup.py
│   ├── start
│   ├── stop
│   ├── update-status
│   └── upgrade-charm
├── icon.svg
├── LICENSE
├── metadata.yaml
├── README.md
├── revision
└── version
```
 Thats about it! The whole charm is now there. Congratulations - you have learned how to retrieve a charm directly from charmstore!
 
 Now, lets deploy it to your cloud! You can decide for your self if you want to deploy from charmstore or from your local version. For juju, its all the same.
 
# Deploy
Lets deploy using the charmstore version.
```bash 
juju deploy cs:~erik-lonroth/tiny-python 
```
... and watch the charm come alive.
```bash 
watch -c juju status --color 
```

# Extra excersise: tiny-bash
The tiny-python charm has a sister charm: [tiny-bash]. This charm deploys very fast, since its not pulling in any python modules etc. It also implements all the hooks that tiny-python does.

You can try it if you like and look at the code if you have time. It takes about 5 minutes.
```bash
juju deploy cs:~erik-lonroth/tiny-bash
```
[hooks-environment]: https://discourse.jujucharms.com/t/the-hook-environment-hook-tools-and-how-hooks-are-run/1047
[Erik Lönroth]: http://eriklonroth.wordpress.com
[part 1]: https://discourse.jujucharms.com/t/tutorial-charm-development-beginner-part-1
[tiny-python]: https://jujucharms.com/new/u/erik-lonroth/tiny-python
[tiny-bash]: https://jujucharms.com/new/u/erik-lonroth/tiny-bash
[hooks]: https://docs.jujucharms.com/2.5/en/reference-charm-hooks
[juju-lxd]: https://docs.jujucharms.com/2.5/en/clouds-lxd