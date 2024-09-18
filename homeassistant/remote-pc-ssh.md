# Remote PC Startup/Shutdown From Home Assistant

Source: [https://www.creatingsmarthome.com/index.php/2022/02/12/guide-start-up-and-shut-down-remote-linux-pc-using-home-assistant/](https://www.creatingsmarthome.com/index.php/2022/02/12/guide-start-up-and-shut-down-remote-linux-pc-using-home-assistant/)

## 1.  Create Remote User
```
sudo adduser homeassistant

sudo visudo

homeassistant ALL=(ALL) NOPASSWD: /sbin/poweroff, /sbin/reboot, /sbin/shutdown
```

## 2.  Setup SSH

Create SSH key:
```
mkdir /config/ssh_keys ssh-keygen -t rsa -f /config/ssh_keys/id_rsa_homeassistant
```

Copy SSH key:
```
ssh-copy-id -i /config/ssh_keys/id_rsa_homeassistant.pub homeassistant@remote_hostname
```

Verify: 
```
ssh -i /config/ssh_keys/id_rsa_homeassistant homeassistant@remote_hostname
```

## 3.  Home Assistant Configuration

Open the shell_commands.yaml in editor, create if not existing, and add following line in it (change the remote_hostname to your remote PC IP address or hostname):

```
turn_off_remote_pc: "ssh -i /config/ssh_keys/id_rsa_homeassistant -o 'StrictHostKeyChecking=no' homeassistant@remote_hostname sudo shutdown -h now"
```

Let’s add support to shell commands in our configuration.yaml file

```
shell_command: !include shell_commands.yaml
```

Now that the shell_commands are in place, let’s create switch that turns on/off the machine (replace host and mac address of your remote machine):

```
switch:
  - platform: wake_on_lan
    name: RemotePC
    host: 192.168.1.100
    mac: 12:34:56:78:90:ab
    turn_off:
      service: shell_command.turn_off_remote_pc
```

And it’s done! Now restart home assistant and add the RemotePC switch in Lovelace UI. The machine should start and turn off when requested.
