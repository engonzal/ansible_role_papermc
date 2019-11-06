# papermc

Installs and configures PaperMC Minecraft on RHEL/CentOS or Debian/Ubuntu servers.  PaperMC is an optimized version of Minecraft server that includes many performance enhancments.

[![Build Status](https://travis-ci.org/engonzal/ansible_role_papermc.svg?branch=master)](https://travis-ci.org/engonzal/ansible_role_papermc)

To-do:

* Add banned players
* Add banned ips
* Clean up server.properties docs and add missing properties

## Requirements

Note that this role requires root.  Ensure become: yes is set at the top level of your playbook or invoke the role in your playbook like:

```yaml
- hosts: papermc_server
  roles:
    - role: engonzal.papermc
      become: yes
```

## Role Variables

While there are no required variables, you may want to consider the amount of memory to allocate.  3G is recommended for 1.14.4 deployments.

### Required

```yaml
# Not required, but if you're running 1.14.4 you should aim for "3G".
papermc_mem: 1G
```

Set the amount of memory to use for your minecraft server.

### Optional

```yaml
papermc_version: "1.14.4"
```

You can specify a specfic version of papermc to install.  Note that you can view available version using the papermc api: <https://papermc.io/api/v1/paper>

```yaml
papermc_dir_main: /app/papermc
```

Set the directory where you would like to install the paper app/configs/world files.

```yaml
papermc_server_port: 25565
papermc_motd: "The SSHCraft Server"
papermc_broadcast_console_to_ops: "false"
```

You can also consigure the server.properties file with some custom parameters.

```yaml
papermc_whitelist:
  - engonzal
papermc_white_list: "true"
papermc_enforce_whitelist: "true"
```

You can also start the server with a whitelist.

```yaml
# Default value
papermc_run_options: "-XX:+UseG1GC -XX:+UnlockExperimentalVMOptions -XX:MaxGCPauseMillis=100 -XX:+DisableExplicitGC -XX:TargetSurvivorRatio=90 -XX:G1NewSizePercent=50 -XX:G1MaxNewSizePercent=80 -XX:G1MixedGCLiveThresholdPercent=35 -XX:+AlwaysPreTouch -XX:+ParallelRefProcEnabled -Dusing.aikars.flags=mcflags.emc.gs"
```

Some custom java parameters can be added.  By default the Aikar params are used: <https://aikar.co/2018/07/02/tuning-the-jvm-g1gc-garbage-collector-flags-for-minecraft/>

If you want to start the server without any custom options set: ```papermc_run_options: ""```

Also take a look at ```default/main.yml``` to see default values.  Server setting defaults can be viewed in tasks/server_settings.yml.

## Example Playbook

### Simple Example (defaults)

```yaml
- hosts: papermc_server
  roles:
      - { role: engonzal.papermc }
```

### Advanced Example (custom server)

```yaml
- hosts: papermc_server
  vars:
    papermc_mem: 1G
    papermc_server_port: 25560
  roles:
      - { role: engonzal.papermc }
```

## License

BSD

## Author Information

This role was created in 2019 by Noe Gonzalez (<http://engonzal.com> and <https://buildahomelab.com>)
