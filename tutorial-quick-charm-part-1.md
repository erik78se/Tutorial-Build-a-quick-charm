# Tutorial: Build a quick charm as a subordinate

Difficulty: Beginner

Author: [Erik Lönroth](http://eriklonroth.wordpress.com)

## What you will learn
This tutorial is a good start for classic sysadmins that wants to approach true first-class DevOps:ing with juju.

We will create a charm that adds a file to the /etc/ directory and installs a package.

Neither knowledge in python, nor reactive programming is needed to complete this tutorial.

You will learn:

* How to build charms using only standard juju [hooks].
* How to add in a file to a charm.
* How to use the juju command: "juju run" to execute remote commands.

## Preparation
1. You should have read about juju [hooks]; especially the 'install' hook as we will implement that hook in this tutorial.

2. You should have completed the first two beginner juju tutorials to get familiar with building, deploying and relating charms.

[Part 1] - First steps developing juju charms

[Part 2] - Adding in functionality with "layers" and connecting to a database.

Lets begin our work.

## Create a "bash" charm
Start by creating a bash charm.

```bash
$ charm create -t bash my-tweaks
INFO: Generating charm for my-tweaks in ./my-tweaks
INFO: No my-tweaks in apt cache; creating an empty charm instead.
```

A stub charm 'my-tweaks' is initiated with all basic hooks added in the "hooks" directory.

Lets examine our charm structure:
```
$ tree my-tweaks
my-tweaks
├── README.ex
├── config.yaml
├── hooks
│   ├── config-changed
│   ├── install
│   ├── relation-name-relation-broken
│   ├── relation-name-relation-changed
│   ├── relation-name-relation-departed
│   ├── relation-name-relation-joined
│   ├── start
│   ├── stop
│   └── upgrade-charm
├── icon.svg
├── metadata.yaml
└── revision
```
In the 'hooks' directory, a few scripts are placed. Those are all refered to as: "hooks" and will be executed by juju at certain times or events.

A good charmer will implement all of the hooks, but we will settle with implementing the "install" hook for now.

You should pay attention to the "relation-name-relation-" prefix for some of the hooks above. In the next part of this tutorial, we will implement the juju-info-relation-joined" which an implicit relation which is always present in juju charms. (Read more about [implicit relations] here).

But to continue, we need to fill in some missing pieces from our 'my-tweaks' charm.

# Clean up the 'my-tweaks' charm
Lets edit the "metadata.yaml" to look like this:
```bash
cd my-tweaks
```
metadata.yaml
```yaml
name: my-tweaks
summary: My tweaks installs and adds
maintainer: My Name <my.name@foo.bar>
description: |
  It does
tags:
  - misc
subordinate: false
series: ['bionic','disco']

```
Fix the README so we don't get so much warnings later on.
```bash
$ rm README.ex
$ echo "# Tutorial charm" > README
```
Good work, lets try build the charm!
```bash
$ charm build
build: Build dir not specified via command-line or environment; defaulting to /tmp/charm-builds
build: Please add a `repo` key to your layer.yaml, with a url from which your layer can be cloned.
build: Destination charm directory: /tmp/charm-builds/my-tweaks
build: The top level layer expects a valid layer.yaml file
build: Processing layer: my-tweaks (from .)
proof: I: `display-name` not provided, add for custom naming in the UI
proof: I: Includes template icon.svg file.
proof: W: no copyright file
proof: I: all charms should provide at least one thing
```
Great! Our charm builds and the resuting charm is placed in "/tmp/charm-builds", but before we can deploy our charm, we need to do some serious fixes for juju to operate properly what we have done.

# OMG fix this
```bash
touch hooks/hook.template
echo "#!/bin/bash > hooks/leader-elected"
echo "#!/bin/bash > hooks/leader-settings-changed"
echo "#!/bin/bash > hooks/update-status"
echo "#!/bin/bash > hooks/install"
chmod +x hooks/*
```
After messing with this, lets continue.

## Implement the 'install' hook
We now need to create/implement the install hook script which is run as part of the charm being installed. There are a few other hooks that are invoked by the juju-agent [link-needed], you might want to have a look at them if you want to explore more of juju. To keep this example small, will only implement the install hook in this tutorial. You would likely do more work in a real scenario depending on your ambition with the charm.
```bash
nano ~/my-tweaks/hooks/install
```
Is should look like this:
```bash
#!/bin/bash
# Hook-tools docs: https://docs.jujucharms.com/2.5/en/reference-hook-tools

status-set maintenance "Installing"
echo "Hello World is always the result" > /etc/hello-world
status-set active "Ready"
```
## Proof, build, deploy, relate
charm proof messes with layer.yaml?
```bash
charm proof
W: cannot parse /home/erik/Tutorial-Build-a-quick-charm/my-tweaks/layer.yaml: 'NoneType' object has no attribute 'get'
```
[hooks]: https://docs.jujucharms.com/2.5/en/authors-charm-hooks
[part 1]: https://discourse.jujucharms.com/t/tutorial-charm-development-beginner-part-1
[part 2]: https://discourse.jujucharms.com/t/tutorial-charm-development-beginner-part-2
[implicit relations]: https://docs.jujucharms.com/2.5/en/authors-relations#implicit-relations
[juju-info]: https://github.com/juju-solutions/interface-juju-info
