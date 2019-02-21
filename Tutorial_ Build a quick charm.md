# Tutorial: Build a quick charm

Difficulty: Beginner
Author: [Erik Lönroth](http://eriklonroth.wordpress.com)

## What you will learn

This guide will go through a way to build a charm using only bash. Neither python, nor reactiveis needed. It is an example of how to use juju to perform a rudimentary sysadmin task as a system admin. Install a package and write a config file.

This example will work on another linux distro aswell, in this example; "centos".

You should have gone through the first two tutorials to have a proper build environment setup for juju charming.

[Part 1] - First steps developing juju charms
[Part 2] - Adding in functionality with "layers" and connecting to a database.

You will learn in this part:

## Create a "bash" charm
Create a new charm with the template "bash"
```bash
    charm -t bash create my-tweaks
```

## Implement the "install" hook
We now need to create/implement the install hook script which is run as part of the charm being installed. There are a few other hooks that are invoked by the juju-agent [link-needed], you might want to have a look at them if you want to explore more of juju. To keep this example small, will only implement the install hook in this tutorial. You would likely do more work in a real scenario depending on your ambition with the charm.
```bash
nano ~/my-tweaks/hooks/install
```
Is should look like this:
```bash
content
content
content
```


## A subordinate charm
To let our charm piggy-back on other charms, we need to turn our charm into a subordinate. This is handy, since we can deploy an already existing charm, and use our subordinate to "tweak" it to test somehting without changing the primary charm. 

To make our charm a subordinate charm:

1. Use the implicit interface;"juju-info" 
2. Add a configuration paramter; "subordinate: true" to metadata.yaml.
3. Implement the juju-info-relation-joined hook.

Doing this, turns our charm into a "subordiate charm" which can be related to any existing application already deployed by juju. A subordinate charm will wait to install until the primary charm is successfully deployed. We will do this last in this tutorial.

Lets perform the three steps.
## Add the interface to metadata.yaml
## Add the configurataion paramter.
## Implement the "juju-info-relation-joined"

## Validating the charm