# MinecraftSystemdUnit
Systemd Unit file for Minecraft Server

## Installation

1. Connect to the server or if you want to run the server on your machine, open a terminal
1. Become root using `su` or `sudo`
    - To validate that you're now root, run `id`
1. Next install the necessary packages 
    ```bash
    apt-get install -y openjdk-11-jre-headless curl screen nano bash grep
    ```
1. Create the `/opt` folder if it doesn't already exist 
    ```bash
    mkdir /opt
    ```
1. Now you need to create the user for the service
    ```bash
    adduser --system --shell /bin/bash --home /opt/minecraft --group minecraft
    chmod +t /opt/minecraft
    ```
1. Create the Systemd Unit file:
    ```bash
    nano /etc/systemd/system/minecraft@.service
    # paste the contents of minecraft@.service
    ```
    or 
    ```bash
    curl https://raw.githubusercontent.com/agowa338/MinecraftSystemdUnit/master/minecraft%40.service > /etc/systemd/system/minecraft@.service
    ```

## Setup Instance

Each server has it's own subdirectory of `/opt/minecraft`. This means that when a new server is being added, the following steps need to be followed:

1. Create a subdirectory of `/opt/minecraft` for your Minecraft server, we'll be using `feed-the-beast` as an example
    - It is recommended to use a name which only includes lowercase characters, spaces should be replaced with dashes
    ```bash
    cd /opt/minecraft
    mkdir feed-the-beast
    ```
1. Upload your files to the `/opt/minecraft/feed-the-beast` directory
    - **Note**: All uploaded files should be owned by the `minecraft` user
    ```bash
    chown -R minecraft:minecraft /opt/minecraft/feed-the-beast
    ```
1. Accept the Minecraft server's EULA
    ```bash
    echo 'eula=true' > eula.txt

### RAM allocation

With `minecraft@.service` it's possible to specify the RAM allocation per server. This can be done using a `server.conf` file in the server's directory.

**Example**

```properties
# /opt/minecraft/ftb-beyond/server.conf
MCMINMEM=512M
MCMAXMEM=2048M
```

### Feed the Beast

Some Feed the Beast packs include a `FTBInstall.sh`. This script needs to be executed before starting the server.

```bash
cd /opt/minecraft/ftb-beyond
su -c 'bash /opt/minecraft/ftb-beyond/FTBInstall.sh' minecraft
```

## Usage

### Enable auto-start Minecraft server on boot

```bash
systemctl enable minecraft@ftb-beyond
```

### Disable auto-start Minecraft server on boot

```bash
systemctl disable minecraft@ftb-beyond
```

### Start Minecraft server manually

```bash
systemctl start minecraft@ftb-beyond
```

### Stop Minecraft server manually

```bash
systemctl stop minecraft@ftb-beyond
```

### Connecting to the Minecraft server console

To enter the console, [`screen`](https://linux.die.net/man/1/screen) is used.

All screen sessions are owned by the `minecraft` user and are prefixed with `mc-`.

```bash
# This will attach to the Minecraft server called `ftb-beyond`
su -c 'screen -r mc-ftb-beyond' minecraft
```

**Note**: To detach (exit) from the session, press <kbd>CTRL + A</kbd> followed by <kbd>D</kbd>.
