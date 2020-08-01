# MinecraftSystemdUnit
Systemd Unit file for Minecraft Server

## Installation
1. Connect to your (v)root server or if you want to run the server on your machine, open a terminal.
1. Become root using su or sudo. To check if you are root run "echo $EUID" if it returns "0" you are root.
1. Next install the necessary packages "apt install openjdk-8-jre-headless curl screen nano bash grep"
1. Create the /opt folder if it doesn't already exist "mkdir /opt"
1. Now you need to create the user for the service: "adduser --system --shell /bin/bash --home /opt/minecraft --group minecraft"
1. Create the Systemd Unit file "nano /etc/systemd/system/minecraft@.service" : "curl https://raw.githubusercontent.com/agowa338/MinecraftSystemdUnit/master/minecraft%40.service > /etc/systemd/system/minecraft@.service"

## Setup Instance
Now you can Upload your FTB Modpacks into an subfolder of /opt/minecraft/. For example you would place the modpack "FTB Beyond" in "/opt/minecraft/FTBBeyond" (without spaces in the name).

If you want to run vanilla instances, just create a folder within /opt/minecraft and upload the minecraft_server.jar and create the eula.txt file (using: echo "eula=true" > /opt/minecraft/vanilla/eula.txt).

After you uploaded the minecraft server files, make sure, that "minecraft" is the owner and owning group. To do so just run `ls -la /opt/minecraft`. If it is not, run `chown minecraft:minecraft /opt/minecraft/FTBBeyond`.
You may also require to complete the installation. For current FTB packages you would run:
```
cd /opt/minecraft/FTBBeyond
echo "eula=true" > /opt/minecraft/FTBBeyond/eula.txt
su -c "/opt/minecraft/FTBBeyond/FTBInstall.sh" -s "/bin/bash" minecraft
```

If you are hosting multiple instances and want to use different RAM setting with each, create file `/opt/minecraft/XX/server.conf` with below contents to override default RAM settings.
```
MCMINMEM=512M
MCMAXMEM=2048M
```

## Usage
### Enable Autostart
```
systemctl enable minecraft@FTBBeyond
```
### Disable Autostart
```
systemctl disable minecraft@FTBBeyond
```
### Start Manually
```
systemctl start minecraft@FTBBeyond
```
### Stop Manually
```
systemctl stop minecraft@FTBBeyond
```
### Enter Server Commands
To enter the console "screen" is used (that's why it is in the script).
**Note**: To detach (exit) screen press [CTRL] + [A] followed by [D].
```
su - minecraft -c "/usr/bin/screen -r" minecraft
```
The command switches to the minecraft screen session using the minecraft user account.

