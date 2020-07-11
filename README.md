# Rdio Scanner on a Raspberry Pi

> DEPRECATED SINCE VERSION 4.3 OF [RDIO SCANNER](https://github.com/chuot/rdio-scanner). THE DOCKER INSTALLATION METHOD IS NOW RECOMMENDED FOR RASPBERRY PI.

The purpose of this script is to simplify the procedure for installing a **fully functional P25 police radio scanner** using [Trunk Recorder](https://github.com/robotastic/trunk-recorder) and [Rdio Scanner](https://github.com/chuot/rdio-scanner) on a **Raspberry Pi**.

If you do not yet know what it is all about, I encourage you to visit each of the sites mentioned above and read a little.

## Minimum Requirements

* One [Raspberry Pi 3 B+](https://www.raspberrypi.org/products/raspberry-pi-3-model-b-plus/) or newer
* One good **RTL-SDR USB dongle** with a **T**emperature **C**ompensated **C**rystal **O**scillator (**TCXO**)
* One relatively fast MicroSD card, faster is better

The installation script was tested on a **new installation** of **Raspbian Buster Lite** with this hardware:

* Raspberry Pi 3 B+
* NooElec NESDR Nano 2
* Samsung EVO Plus 32GB MicroSDXC

## Installation Procedure

### Step 1: Raspbian Installation

Start with a fresh install of **Raspbian Buster Lite**:

* Install [Raspbian Buster Lite](https://www.raspberrypi.org/downloads/raspbian/) according to the [installation guide](https://www.raspberrypi.org/documentation/installation/installing-images/README.md).
* If you prefer a headless installation type, follow these [instructions](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md)
* Use the [raspi-config](https://www.raspberrypi.org/documentation/configuration/raspi-config.md) for at least:
  * Change *pi* user password
  * Localisation options
  * Network hostname
* Update *Raspbian* by following these [instructions](https://www.raspberrypi.org/documentation/raspbian/updating.md)

### Step 2: Installation Script

The installation process should take approximately **100 minutes** on a system similar to the one mentioned above.

Step 2.1: Install *git*.

```bash
pi@raspberrypi:~ $ sudo apt install -y git
```

Step 2.2: Clone the [repository](https://github.com/chuot/rdio-scanner-pi-setup)

```bash
pi@raspberrypi:~ $ git clone https://github.com/chuot/rdio-scanner-pi-setup.git
```

Step 2.3: Launch the script

```bash
pi@raspberrypi:~ $ cd rdio-scanner-pi-setup
pi@raspberrypi:~/rdio-scanner-pi-setup $ ./rdio-scanner-pi-setup
```

Note that the script can be run over and over again if something is wrong (but it shouldn't happen).

### Step 3: Post Installation

Everything is configured to start automatically when the Raspberry Pi has started. However, the start of Trunk Recorder is delayed by one minute to allow the RTL-SDR dongle to warm up a bit before going live.

You can validate your new installation by loading *Rdio Scanner* on your browser.

At this point, you can get rid of the installation repository:

```bash
pi@raspberrypi:~/rdio-scanner-pi-setup $ cd
pi@raspberrypi:~ $ rm -fr rdio-scanner-pi-setup
```

The preinstalled *Trunk Recorder* configuration files should be modified to suit your needs. Please refer to the [Trunk Recorder documentation](https://github.com/robotastic/trunk-recorder/blob/master/README.md).

**PLEASE REMEMBER TO CHANGE THE RADIO USER'S PASSWORD FROM THE DEFAULT**

## FAQ

**Q: How do I see *Trunk Recorder* logs?**\
**A: `journalctl -u trunk-recorder.service -f`**

**Q: How do I see *Rdio Scanner* logs?**\
**A: `journalctl -u rdio-scanner.service -f`**

**Q: How do I restart/start/stop *Trunk Recorder*?**\
**A: `sudo systemctl restart -u trunk-recorder.service`**\
**A: `sudo systemctl start -u trunk-recorder.service`**\
**A: `sudo systemctl stop -u trunk-recorder.service`**

**Q: How do I restart/start/stop *Rdio Scanner*?**\
**A: `sudo systemctl restart -u rdio-scanner.service`**\
**A: `sudo systemctl start -u rdio-scanner.service`**\
**A: `sudo systemctl stop -u rdio-scanner.service`**

**Q: Where is *Trunk Recorder*'s configuration?**\
**A: `/home/radio/trunk-recorder`**

**Q: Where is *Rdio Scanner*'s configuration?**\
**A: `/home/radio/rdio-scanner/server/.env`**

**Q: Should I modify *Trunk Recorder* script files?**\
**A: Absolutely, but read the appropriate documentation to understand what you are doing**

**Q: How do I test my *Trunk Recorder*'s new configuration?**\
**A: Do the following:**

```bash
radio@raspberrypi:~ $ sudo systemctl stop trunk-recorder.service
radio@raspberrypi:~ $ cd trunk-recorder
radio@raspberrypi:~/trunk-recorder $ ./recorder
```

  Then `CTRL-C`, change things and `./recorder` again.

  When you done and happy with the results,

```bash
radio@raspberrypi:~/trunk-recorder $ sudo systemctl start trunk-recorder.service
radio@raspberrypi:~/trunk-recorder $ journalctl -u trunk-recorder.service -f
```

**Q: How to load updated/new `talkgroups.csv` to *Rdio Scanner*?**\
**A: Do the following:**

```bash
radio@raspberrypi:~ $ cd trunk-recorder
radio@raspberrypi:~/trunk-recorder $ ./script/upload-systems
```

**Q: How do I update *Trunk Recorder***\
**A: As the user *radio*, do the following:**

```bash
radio@raspberrypi:~ $ cd src/trunk-recorder
radio@raspberrypi:~/src/trunk-recorder $ git pull
...
radio@raspberrypi:~/src/trunk-recorder $ make
...
radio@raspberrypi:~/src/trunk-recorder $ sudo systemctl stop trunk-recorder
radio@raspberrypi:~/src/trunk-recorder $ cp recorder ~/trunk-recorder
radio@raspberrypi:~/src/trunk-recorder $ sudo systemctl start trunk-recorder
```

**Q: How do I update *Rdio Scanner***\
**A: As the user *radio*, do the following:**

```bash
radio@raspberrypi:~ $ cd rdio-scanner
radio@raspberrypi:~/rdio-scanner $ npm run update
```

**Q: I need help, it's not working!**\
**A: Read [Trunk Recorder's documentation](https://github.com/robotastic/trunk-recorder/blob/master/README.md)**\
**A: Ask your questions over [Trunk Recorder's Gitter](https://gitter.im/trunk-recorder/Lobby)**\
**A: Read [Rdio Scanner's documentation](https://github.com/chuot/rdio-scanner/blob/master/README.md)**\
**A: Ask your questions over [Rdio Scanner's Gitter](https://gitter.im/rdio-scanner/Lobby)**