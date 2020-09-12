# Home Assistant Configurator
_Formally known as [hass-configurator](https://github.com/danielperna84/hass-configurator)_

The HASS-Configurator is a small webapp (you access it via web browser) that provides a filesystem-browser and text-editor to modify files on the machine the configurator is running on. It has been created to allow easy configuration of Home Assistant. It is powered by Ace editor, which supports syntax highlighting for various code/markup languages. YAML files (the default language for Home Assistant configuration files) will be automatically checked for syntax errors while editing.

Read the full documentation at [github](https://github.com/danielperna84/hass-configurator).

[![Get it from the Snap Store](https://snapcraft.io/static/images/badges/en/snap-store-black.svg)](https://snapcraft.io/home-assistant-snap)
[![Donate with PayPal](https://giaever.online/paypal-donate-button.png)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=69NA8SXXFBDBN&source=https://git.giaever.org/joachimmg/home-assistant-snap)

## Build and installation
### Install from The Snap Store (Recommended)

Make sure you have Snapd installed on your system. See [Installing snapd](https://snapcraft.io/docs/installing-snapd) for a list of distributions with and without snap pre-installed, including installation instructions for those that have not.

```bash
$ snap install home-assistant-configurator
```

The snap `home-assistant-snap` is required and will automatically install if it isn't already, when installing `home-assistant-configurator`.

#### Configuration

See `snap get home-assistant-configurator -d` for available options. The options reflects the options described at the [configuration](https://github.com/danielperna84/hass-configurator/wiki/Configuration)-section on github, but with a different naming/mapping. The naming/mapping strategy is easily recoginizable.

#### Connect with Home Assistant

Make sure that the connection is established (should be automatical), by executing

```
snap connections
```

If the connection is not in the returned list, execute

```
snap connect home-assistant-configurator:configurations home-assistant-snap:configurations
```
to make the connection. Now add the following to your home-assistant-snap configuration.yaml-file:

```
panel_iframe:
  configurator:
    title: Configurator
    icon: mdi:wrench
    url: https://domain.tld.or.ip:3218
```
(the url can be local)

Restart Home Assistant (`snap restart home-assistant-snap.hass`) and you should have a menu entry to the left called «Configurator» wit a wrench icon.

### Build this snap from source

We recommend that your download a pre-built version of this snap from [The Snap Store](https://snapcraft.io/home-assistant-configurator), or at least make sure that you checkout the latest tag, as the master tag might contain faulty code during development.

1. **Clone this repo and checkout the latest tag**

```bash
$ git clone https://git.giaever.org/joachimmg/home-assistant-configurator.git

# Go into directory
$ cd ./home-assistant-configurator

# Checkout tag
$ git checkout <tag>
```
_**NOTE**: You can find the latest tag with `git ls-remote --tags origin`_

2. **Build and install**

Make sure you have snapd (see [Installing snapd](https://snapcraft.io/docs/installing-snapd)) and latest version of Snapcraft. Install Snapcraft with

```bash
$ sudo snap install snapcraft --classic
```

Or update with

```bash
$ sudo snap refresh snapcraft
```

2.2 **With multipass**

From the «home-assistant-configurator»-directory, run

```bash
$ snapcraft
```

Multipass will be installed and a virtual machine will boot up and build your snap. The final snap will be located in the same directory.

2.3 **With LXD** (*required* for Raspberry Pie)

Snapcraft will try to install multiplass and build the snap for you, but on *Raspberry Pi* it will fail. You will have to use an LXD container.

Install LXD and create a container

```bash
$ snap install lxd
$ snap lxd init
```

Make sure your user is a member of lxd-group

```bash
$ sudo adduser $USER lxd
```

Launch an Ubuntu 20.04 container instance

```bash
$ lxc launch ubuntu:20.04 home-assistant-configurator
```

Make sure you're in the «home-assistant-configurator»-directory and go into the shell of your newly created container

```bash
$ lxc exec -- home-assistant-configurator /bin/bash
```

and run

```bash
$ SNAPCRAFT_BUILD_ENVIRONMENT=host snapcraft
```

when the build is complete, you'll have to exit the shell and pull the snap-file from the container. See `lxc file pull --help`.

3. **Install new built snap**

```
$ sudo snap install ./home-assistant-configurator_<source-tag>.snap --dangerous
```

Now, [connect hass-configurator to Home Assistant](#connect-with-home-assistant).
