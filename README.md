# iocage-homeassistant
Artifact file(s) for [Home-Assistant](https://www.home-assistant.io/) + [App-Daemon](https://www.home-assistant.io/docs/ecosystem/appdaemon/) / [HA-Dashboard](https://www.home-assistant.io/docs/ecosystem/hadashboard/) + [Hass-Configurator](https://www.home-assistant.io/docs/ecosystem/hass-configurator/#configuration-ui-for-home-assistant)

- This branch is for FreeNAS 11.3

If you are using a Z-Wave or Zigbee controller such as the Aeotec Gen-5, Nortek HUSBZB-1, or similiar USB device, you will need to create a custom devfs_ruleset on the FreeNAS host system. The steps for this as well as setting up a separate dataset for the Home Assistant configuration files and additional information for using this repo can be found in this [FreeNAS Resource](https://forums.freenas.org/index.php?resources/fn-11-2-iocage-home-assistant-jail-plugins-for-node-red-mosquitto-amazon-dash-tasmoadmin.102/)

---
---
## iocage-jail-homeassistant

 - This script can be used to create an iocage-jail for Home-Assistant
 - Includes option to install App-Daemon/HA-Dashboard and(or) Hass-Configurator

**Download pkg-list and create a jail using it to install requirements**

    wget -O /tmp/pkglist.json https://gist.githubusercontent.com/tprelog/99b8177aefba1871ad5adc34dbbfb2a6/raw/b7022796d810505af1ba0cc0c2fc435b92e801ff/homeassistant.json
    sudo iocage create -r 11.3-RELEASE dhcp=1 bpf=yes vnet=on boot=on allow_raw_sockets=1 -p /tmp/pkglist.json -n homeassistant

**Optional: mount a dataset inside the jail**

    sudo iocage fstab -a homeassistant "/mnt/tank/user/hass /home/hass nullfs rw 0 0"

**Git script and begin install**

    sudo iocage exec homeassistant git clone -b 11.3-RELEASE https://github.com/tprelog/iocage-homeassistant.git /root/.iocage-homeassistant
    sudo iocage exec homeassistant bash /root/.iocage-homeassistant/post_install.sh standard

**Answer questions will choose what gets installed**

    Install Home-Assistant?  [Y/n]:
    Install Hass-Configurator?  [Y/n]:
    App-Daemon & HA-Dashboard?  [Y/n]:
    Use the pre-configured examples?  [Y/n]:

***Profit!***

**Includes a simple console menu for updates**

    sudo iocage exec homeassistant bash update
    Password:

    1) Home Assistant  3) Configurator    5) Status
    2) App Daemon      4) FreeBSD         6) Exit
    Enter Number to Upgrade:

---

  - Home Assistant: `http://YOUR.HOMEASSISTANT.IP.ADDRESS:8123`
  - HADashboard   : `http://YOUR.HOMEASSISTANT.IP.ADDRESS:5050`
  - Configurator  : `http://YOUR.HOMEASSISTANT.IP.ADDRESS:3218`

---
---

#### iocage-plugin-homeassistant

 - This script can also be used to create an iocage-plugin for Home Assistant on FreeNAS 11.3
 - I recommend using this script to install a standard iocage-jail for Home Assistant as shown above

**Download plugin and install**

    wget -O /tmp/homeassistant.json https://raw.githubusercontent.com/tprelog/iocage-homeassistant/11.3-RELEASE/homeassistant.json
    sudo iocage fetch -P /tmp/homeassistant.json

---

###### To see a list of jails as well as their ip address

    sudo iocage list -l
    +-----+-----------------+------+-------+----------+-----------------+---------------------+-----+----------+
    | JID |      NAME       | BOOT | STATE |   TYPE   |     RELEASE     |         IP4         | IP6 | TEMPLATE |
    +=====+=================+======+=======+==========+=================+=====================+=====+==========+
    | 74  | homeassistant   | on   | up    | jail     | 11.3-RELEASE-p5 | epair0b|192.0.1.75  | -   | -        |
    +-----+-----------------+------+-------+----------+-----------------+---------------------+-----+----------+
    | 76  | homeassistant_2 | on   | up    | pluginv2 | 11.3-RELEASE-p5 | epair0b|192.0.1.77  | -   | -        |
    +-----+-----------------+------+-------+----------+-----------------+---------------------+-----+----------+

- Tested on FreeNAS-11..3-RC1
- More information about [iocage plugins](https://doc.freenas.org/11.3/plugins.html) and [iocage jails](https://doc.freenas.org/11.3/jails.html) can be found in the [FreeNAS guide](https://doc.freenas.org/11.3/intro.html#introduction)
