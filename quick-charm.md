# Quick-charm

# Tutorial: A quick charm

Difficulty: Beginner
Author: [Erik Lönroth]

## What you will learn
This tutorial is a good start to learn about the concept of "juju hooks".

You will:

* Learn how to retrieve a charm from [charmstore].
* Learn how to use juju [hooks] in charms.
* Get an overview about the juju event-loop / state-machine.
* Learn how to use the juju command: 'juju run' to execute remote commands.
* Learn how to use 'juju debug-log' for debuging your charms.

Lets get started!
# Preparations
1. You need an internet connection and a linux client.
1. You need a juju controller to deploy charms through. Here is how you can get that running on your local machine: [juju-lxd]
1. You need a basic charm developer environment. If you have completed [part 1] "Setup up a basic workbench", then your'e all set.

# The "tiny-python" charm
I have prepared a charm already for you. Its called "[tiny-python]". We will modify tiny-python during this tutorial to learn about juju:s "hooks". 

Your first task is to download/pull the tiny-python charm. Go ahead and run:

```bash
charm pull cs:~erik-lonroth/tiny-python
```
This command downloads the latest version of [tiny-python] from the charmstore to your local filesystem. This can be done with any charm.

Lets look at what we got.
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
 
 Now, lets try deploy it to your cloud! You can decide if you want to deploy from charmstore or from your local version. For juju, its all the same.
 
# juju deploy, status and debug-log
Lets deploy the charmstore version of the charm and follow it through:
```bash 
juju deploy cs:~erik-lonroth/tiny-python 
juju status
juju debug-log
```
'juju debug-log' lets us see from a debug point of view what happens inside the model.

Once you are happy watching logs and status. The application will enter the 'active' status and workload. It all seems good and we can continue.

# Remove the App/charm
Lets clean up, so we don't run into problems later.
```
juju remove application tiny-python
```
Now, your juju model should be empty.

# Juju state machine
Before we dive deeper into "hooks", you need to understand when hooks are triggered by the [juju-state-machine]. There are many hooks, but a few are core to the operation of all charms.

The core hooks are the following: 
* install
* config-changed
* start
* leader-elected
* leader-settings-changed
* start
* stop
* update-status

The tiny-python charm implements all "core hooks" (which is good practice) but a charm doesn't strictly need to implement any of them to be able to deploy.

Now that you have an initial understanding about the core hooks, lets explore the "install" hook to get our hands dirty.

# Modifying the 'install' hook
Lets decide that our charm will install the "hello" snap and set application version from the yaml output of 'snap info hello'.

Edit the 'install' hook as below.
```python
import setup
setup.pre_install()

from charmhelpers.core import hookenv
import subprocess
import yaml

def install():
    subprocess.call(['snap', 'install', 'hello', '--stable'])
    output = subprocess.check_output(['snap', 'info', 'hello'])
    yamldata = yaml.load(output)
    hookenv.application_version_set( yamldata['installed'].split(" ")[0] )
    hookenv.status_set('maintenance', 'Installed, waiting for start')
    hookenv.log('Did some installing', 'INFO')

if __name__ == "__main__":
    install()

```

# Deploy our local version
With our changes saved, lets deploy our modified local charm.

```bash
juju deploy ./tiny-python
watch -c juju status --color
juju debug-log
```
Amazing! We now see what version of "hello" we have deployed!

See the Store for our App is set to "local"? This tells us that we are using a local version of the charm, instead the one from charmstore.

# The update-status hook & juju run
Lets now turn our eyes a bit towards the update-status hook. This hook is triggered periodically as you can see in the [juju-state-machine]. Its a great place to gather information or evaluate the status of your application. The interval at which this update-status occurs is however several minutes. You dont have that time when you are debugging. We need a means to trigger it to run when you want it to.

This is where ```juju run``` comes in.

Lets use juju run to execute the update-status script:

```
juju run --unit tiny-python/1 'hooks/update-status'
```

As you can see from the output from ```juju status```, we have forced the update-status script to run.

We can play with any other unix commands aswell, lets find out the hostname of machine 1:
```juju run --machine 1 hostname```

The 'juju run' command as a fantastic tools in situations where you don't have the ability to use ssh! Go ahead and run some other unix commands of you choise.

You excercise now is to modify the 'update-status' hook to update the application version. Its really just to copy the code from the install hook and I will leave that to you before we continue and learn how to upgrade your juju charms in your models.

# The upgrade-charm hook
Once you have made changes to your charm, you want to upgrade them. You can either remove your charm altogether (juju remove-application) and start new, but you then need to wait for a new machine to come alive. This is not what you want always. Also, the "install" hook might take a long time to run.

With your changes in the "update-status" hook, lets use "juju upgrade-charm" to upgrade the charm.

```bash
juju upgrade-charm tiny-python --path=./tiny-python
```
If you have done everything right, you will now see a new 'Rev' number in the model. If it fails, you can probably find out your problem with:
```
juju debug-log
```

What a glorious achievement! Its now up to you to explore what you can do with all the other hooks available to you! Only your imagination is the limit!

# Extra material : tiny-bash
The tiny-python charm has a sister charm: [tiny-bash]. This charm deploys very fast, since its not pulling in any python modules etc. It also implements all the hooks that tiny-python does.

You can try it if you like and look at the code if you have time. It takes about 5 minutes and a great spot to test out stuff with hooks.

```bash
charm pull cs:~erik-lonroth/tiny-bash
```

Happy charming!

[hooks-environment]: https://discourse.jujucharms.com/t/the-hook-environment-hook-tools-and-how-hooks-are-run/1047
[Erik Lönroth]: http://eriklonroth.wordpress.com
[part 1]: https://discourse.jujucharms.com/t/tutorial-charm-development-beginner-part-1
[tiny-python]: https://jujucharms.com/new/u/erik-lonroth/tiny-python
[tiny-bash]: https://jujucharms.com/new/u/erik-lonroth/tiny-bash
[hooks]: https://docs.jujucharms.com/2.5/en/reference-charm-hooks
[juju-lxd]: https://docs.jujucharms.com/2.5/en/clouds-lxd
[juju-state-machine]: https://github.com/erik78se/Tutorial-Build-a-quick-charm/blob/master/juju-core-state-machine.png
[charmstore]: https://jujucharms.com