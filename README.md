# vpn connect

> Replicate the basic process on Linux that would be done to open a Cisco connection on Windows and then establish a Remote Desktop connection.

## Tested On
Ubuntu 16.04.2 LTS

## Requirements
* remmina: Remote Desktop Client
* ifconfig: network configuration
* openvpn: secure IP tunnel daemon
* openconnect: Cisco AnyConnect VPN
## Setup
1. configure remmina connection
2. configure openconnect
3. place vpn-connect script in ~/bin
4. give script permissions: chmod 755 vpn-connect
5. add ~/bin to the environment variables' PATH
6. add the following to environment variables or replace in script:
    * $WORK_SERVER: server url
    * $WORK_GROUP: security group
    * $WORK_REMMINA_CONNECTION: path to connection file (most likely located in ~/.remmina)

## Usage
1. open terminal (Ctrl+Alt+t)
2. type vpn-connect and enter.
    * this starts the vpn connection similar to a Cisco connection in Windows
3. open 2nd terminal (Ctrl+Alt+t)
4. type vpn-connect -r
    * this establishes the remmina connection similar to a Remote Desktop connection in Windows
5. when done with connection, disconnect the remmina connection via the GUI
6. if the remmina connection terminal has not returned to the prompt, type Ctrl+c
7. in the vpn connection terminal window type Ctrl+c
8. finally, type vpn-connect -d to disconnect the vpn connection
