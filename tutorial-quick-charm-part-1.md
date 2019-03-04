# Tutorial: Build a quick charm as a subordinate

Difficulty: Beginner

Author: [Erik Lönroth](http://eriklonroth.wordpress.com)

## What you will learn
This tutorial is a good start for classic sysadmins that wants to approach true first-class DevOps:ing with juju.

We will create a charm that adds a file to the /etc/ directory and installs a package.

Neither knowledge in python, nor reactive programming is needed to complete this tutorial.

* You will learn how to build charms using only standard juju [hooks].
* You will learn about the juju command: "juju run" to execute remote commands.

As you take on this tutorial, you will likely figure out better ways to do this as they progress, but this is another good start in a journey towards juju mastery.

## Preparation
1. You should have read about juju [hooks], especially the 'install' hook as we will implement that hook in this tutorial.

2. You should have completed the first two beginner juju tutorials to get familiar with building, deploying and relating charms.

[Part 1] - First steps developing juju charms

[Part 2] - Adding in functionality with "layers" and connecting to a database.

Now, lets begin by creating the charm.

## Create a "bash" charm
```bash
$ charm create -t bash my-tweaks
INFO: Generating charm for my-tweaks in ./my-tweaks
INFO: No my-tweaks in apt cache; creating an empty charm instead.
```

We have now created a stub charm 'my-tweaks' with all the basic hooks added in to it.

This is how it looks:
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

We will implement the "install" hook in this tutorial. 

You should pay some attention to the "relation-name-relation-" prefix for some of the hooks above. In the next part of this tutorial, we will implement the juju-info-relation-joined" which an implicit relation which is always present in juju charms. (Read more about [implicit relations] here).

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
## Implement the 'install' hook
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
## Implement the "juju-info-relation-joined" hook
Rename files, add, fix, bla blabla

## Proof, build, deploy, relate

[hooks]: https://docs.jujucharms.com/2.5/en/authors-charm-hooks
[part 1]: https://discourse.jujucharms.com/t/tutorial-charm-development-beginner-part-1
[part 2]: https://discourse.jujucharms.com/t/tutorial-charm-development-beginner-part-2
[implicit relations]: https://docs.jujucharms.com/2.5/en/authors-relations#implicit-relations
[juju-info]: https://github.com/juju-solutions/interface-juju-info
