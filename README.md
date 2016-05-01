# OpenHAB

Install and configure the OpenHAB (www.openhab.org) software for Home
Automation on to Debian-based distros following the instructions at
https://github.com/openhab/openhab/wiki/Linux---OS-X#apt-get

Note: This will run on Rasbian as well as Ubuntu and stock Debian.

## Requirements

To ensure that IP Address Ranges are mapped to their correct CIDR equivalents,
you must install "netaddr" on the device that you use to run this Ansible role.

This is easily done from PIP with the following command on Linux:

```pip install netaddr```

## Usage

### Bindings, Actions, Persistence and IO Plugins

To install and configure bindings, you need to set variables either at a group,
host or role level.

Each section of the variables is named after the configuration file section
name, for example, the following installs and configures the "pushover"
action, the rrd4j persistence layer and the lightwaverf and milight bindings:

```
openhab_actions:
  pushover:
    - { key: defaultToken, value: pushover_token }
    - { key: defaultUser, value: pushover_user_token }
openhab_persistence:
  rrd4j:
    - { key: der5min.def, value: "DERIVE,900,0,U,300" }
    - { key: der5min.archives, value: "AVERAGE,0.5,1,365:AVERAGE,0.5,7,300"}
    - { key: der5min.items, value: "Speedtest*,LW_WifiL*"}
openhab_bindings:
  milight:
    - { key: "milight1.host", value: "192.168.1.102" }
    - { key: "milight1.port", value: "8899" }
  plex:
    - { key: host, value: 192.168.1.104 }
    - { key: port, value: 32400 }
    - { key: refresh, value: 5000 }
  lightwaverf:
    - { key: ip, value: 192.168.1.103 }
    - { key: receiveport, value: 9761 }
    - { key: sendport, value: 9760 }
    - { key: registeronstartup, value: true }
    - { key: senddelay, value: 2000 }
    - { key: okTimeout, value: 1000 }
```

Note that the format is:

```
openhab_<section_type>:
  <action/binding/persistence_name>:
    - { key: key_name, value: value_of_variable }
```

### Sitemaps, rules, and other configuration files

I've not found a way to automate this nicely at the present time, so for now,
create the files as you would if installing manually and place them in the
relevant directory under `files`.  They will automatically be copied in to
place.

## Dependencies

Make sure that you have a role that installs Java.  People appear to have
widely differing opinions on which JVM should be installed (Oracle/OpenJDK etc)
so I have left this choice up to you!

## Example Playbook

This playbook can be run in isolation, however, I would recommend that you
ensure your baseline play installs and configures a firewall and that you
enable the `openhab_use_firewall` variable to make your installation more
secure.

To run the role, create a play with the following content:

    - hosts: openhab_servers
      roles:
         - { role: <your_java_role> }
         - { role: openhab }

## Contributing

Fork the code, make your changes and submit a pull request.  Tests are
available using the kitchen-ec2 plugin, however you will need to configure your
test kitchen setup using the "Dynamic Configuration Instructions" from
http://kitchen.ci/docs/getting-started/dynamic-configuration in order to use it
properly.

## License

MIT

## Author Information

Matthew Macdonald-Wallace
http://doics.co
http://hackerdads.wordpress.com
