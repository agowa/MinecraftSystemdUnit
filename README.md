# MinecraftSystemdUnit
Systemd Unit file for Minecraft Server

## Installation
1. Connect to your (v)root server or if you want to run the server on your machine, open a terminal.<br />
2. Become root using su or sudo. To check if you are root run "echo $EUID" if it returns "0" you are root.<br />
3. Next install the necessary packages "apt install openjdk-8-jre-headless curl screen nano bash grep"<br />
4. Create the /opt folder if it doesn't already exist "mkdir /opt"<br />
5. Now you need to create the user for the service: "adduser --system --shell /bin/bash --home /opt/minecraft --group minecraft"<br />
6. Create the Systemd Unit file "nano /etc/systemd/system/minecraft@.service" : "curl https://raw.githubusercontent.com/agowa338/MinecraftSystemdUnit/master/minecraft%40.service > /etc/systemd/system/minecraft@.service"<br />

## Setup Instance
Now you can Upload your FTB Modpacks into an subfolder of /opt/minecraft/. For example you would place the modpack "FTB Beyond" in "/opt/minecraft/FTBBeyond" (without spaces in the name).<br />
<br />
If you want to run vanilla instances, just create a folder within /opt/minecraft and upload the minecraft_server.jar and create the eula.txt file (using: echo "eula=true" > /opt/minecraft/vanilla/eula.txt).<br />
<br />
After you uploaded the minecraft server files, make sure, that "minecraft" is the owner and owning group. To do so just run "ls -la /opt/minecraft". If it is not, run "chown minecraft:minecraft /opt/minecraft/FTBBeyond".<br />
You may also require to complete the installation. For current FTB packages you would run:
<pre>
cd /opt/minecraft/FTBBeyond
echo "eula=true" > /opt/minecraft/FTBBeyond/eula.txt
su -c "/opt/minecraft/FTBBeyond/FTBInstall.sh" -s "/bin/bash" minecraft
</pre>

## Usage
### Enable Autostart
<pre>systemctl enable minecraft@FTBBeyond</pre>
### Disable Autostart
<pre>systemctl disable minecraft@FTBBeyond</pre>
### Start Manually
<pre>systemctl start minecraft@FTBBeyond</pre>
### Stop Manually
<pre>systemctl stop minecraft@FTBBeyond</pre>
### Enter Server Commands
To enter the console "screen" is used (that's why it is in the script).<br />
<b>Note</b>: To detach (exit) screen press [STRG] + [A] followed by [D].<br />
<pre>
su - minecraft -c "/usr/bin/screen -r" minecraft
</pre>
The command switches to the minecraft screen session using the minecraft user account.<br />

