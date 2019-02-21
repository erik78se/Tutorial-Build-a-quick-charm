# Tutorial: Build a quick charm as a subordinate

Difficulty: Beginner

Author: [Erik Lönroth](http://eriklonroth.wordpress.com)

## What you will learn
* You will learn about subordinate charms and the implicit interface juju-info.
* You will learn how to build a charm using only bash & [hooks], which means neither knowledge in python, nor reactive programming is needed to complete this tutorial.
* To make the tutorial a bit more spicy, we will make the charm also compatible with the linux distribution centos.

This tutorial a great start for sysadmins getting started with juju and DevOps:ing. It mimics tasks as installing a packages, executing system commands or writing config files - as a supplement to deploying full scale systems with juju.

Skilled sysadmins will likely figure out better ways to do this as they progress, but this is a start in a journey towards juju mastery.

## Preparation
You should have completed the first two juju tutorials to get familiar with building, deploying and relating charms. This tutorial assumes you have completed them.

[Part 1] - First steps developing juju charms
[Part 2] - Adding in functionality with "layers" and connecting to a database.

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

To make our charm a subordinate charm we need to perform 4 things:

1. Use the implicit interface;"juju-info" 
2. Add a configuration paramter; "subordinate: true" to metadata.yaml.
3. Implement the 'install' hook.
4. Implement the 'juju-info-relation-joined' hook.

This turns our charm into a "subordiate charm" which then can be related to any existing application already deployed by juju. A subordinate charm will wait to install until the primary charm is successfully deployed. We will do this last in this tutorial.

Lets begin performing the steps.
## Add juju-info to metadata.yaml
## Add the subordinate configurataion parameter.
## Implement the "juju-info-relation-joined" hook
## Implement the 'install' hook

## Proof, build, deploy, relate

[hooks]: https://docs.jujucharms.com/2.5/en/authors-charm-hooks