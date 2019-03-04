# Tutorial: Quick charm (part 2/2)

Difficulty: Beginner

Author: [Erik Lönroth](http://eriklonroth.wordpress.com)

## What you will learn
Building on the previous part of this tutorial, we will turn our charm into a "subordinate" charm. This allows us to add our functionality to already deployed charms on servers.

* You will learn about subordinate charms.
* You will learn how to implement hooks that triggers charms getting joined on specific interfaces.
* You will learn about the interface; [juju-info].

## Preparation
* You should have completed [Quick charm (part 1/2)](tutorial-quick-charm-part-1.md).
* You should have read about [juju relations].

## A subordinate charm

You should pay some attention to the "relation-name-relation-" prefix for some of the hooks above. 

Will implement the juju-info-relation-joined" which an implicit relation which is always present in juju charms. (Read more about [implicit relations] here).

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
[tutorial-charm-development-beginner-part-1]: https://discourse.jujucharms.com/t/tutorial-charm-development-beginner-part-1
[tutorial-charm-development-beginner-part-1]: https://discourse.jujucharms.com/t/tutorial-charm-development-beginner-part-2
[implicit relations]: https://docs.jujucharms.com/2.5/en/authors-relations#implicit-relations
[juju relations]: https://docs.jujucharms.com/2.5/en/authors-relations
[juju-info]: https://github.com/juju-solutions/interface-juju-info
