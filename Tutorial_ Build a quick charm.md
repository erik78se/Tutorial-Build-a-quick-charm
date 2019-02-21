# Tutorial: Build a quick charm

Difficulty: Beginner
Author: [Erik Lönroth](http://eriklonroth.wordpress.com)

## What you will learn

This guide will go through a fast way to build a charm to install a package without using neither python, nor reactive. It serves as an example of how you can use juju to perform rudimentary tasks as a system admin without having to know too much about the juju framework and still be able to use it in you own context.

It will also work on the linux distro "CentOS", which is neat.

You should have gone through the first two tutorials here to get to know how to setup a proper build environment for juju.

[Part 1] - First steps developing juju charms
[Part 2] - Adding in functionality with "layers" and connecting to a database.

You will learn in this part:

* Preparing & setup of a basic workbench.
* Creating the example charm with "charm tools".
* Understanding the anatomy of a charm (files and directories).
* Validating & building the charm.
* Adding functionality via a secondary layer (layer:apt).
* Deploying the example charm with juju.

## Setup up a basic workbench

This is how a typical workbench looks like:

  - **A Juju controller**: To deploy developed charms to. You can [start here][getting-started] to get one up and running.
  - **Python 3.x**: We use python 3 in this tutorial to develop our charm.
  - **Charm Tools**: To create skeleton charms, build, fetch and test charms. [See the Charm Tools page][charm-tools]
  for installation instructions.

  - Three directories for our build environment needs to be created.
```bash
    mkdir -p ~/charms
    mkdir -p ~/charms/layers
    mkdir -p ~/charms/interfaces
```
  - Put these environment variables in your **~/.bashrc**
```bash
    export JUJU_REPOSITORY=$HOME/charms
    export LAYER_PATH=$JUJU_REPOSITORY/layers
    export INTERFACE_PATH=$JUJU_REPOSITORY/interfaces
```
  - Finally source your ~/.bashrc to get the environment properly setup.
```bash
    source ~/.bashrc
```

## Creating the example charm with "charm tools"

To simplify creation of new charms, charmtools exist for us. Lets start a new charm that we name: "layer-example".

```bash
cd ~/charms/layers
charm create layer-example
```
Great work, lets move on to understand what a charm consists of.

## The anatomy of a charm

A bare minimum charm consists of a directory with the charm name and two files: 'layers.yaml' and 'metadata.yaml'. Thats all that is strictly required for a charm to be valid. We do however normally create a directory called 'reactive' where we put a python module named with our charm. This is what happened when we ran 'charm create layer-example' above.

Lets examine what was created.

```
layer-example
├── config.yaml             <-- Configuration options for our charm/layer.
├── icon.svg                <-- A nice icon for our charm.
├── layer.yaml              <-- The layers and interfaces we include.
├── metadata.yaml           <-- Information about our charm
├── reactive                <-- Needed for all reactive charms
│   └── layer_example.py    <-- The charm code
├── README.ex               <-- README
└── tests                   <-- Tests goes in here
    ├── 00-setup            <-- A skeleton setup test
    └── 10-deploy           <-- A skeleton deploy test
```

Note! Prefixing the charm directory name with 'layer-' is a naming convention. It tells us that this charm is a 'reactive' charm. You can read even more on this topic in the official documentation here: [Charms.Reactive]

## Validating the charm