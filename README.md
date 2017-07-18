# Product
work_vpn

> Replicate the basic process on Linux that would be done to open a Cisco connection on Windows and then establish a Remote Desktop connection.

### Tested On
Ubuntu 16.04.2 LTS

### Requirements
* remmina: Remote Desktop Client
* ifconfig: network configuration
* openvpn: secure IP tunnel daemon
* openconnect: Cisco AnyConnect VPN
### Setup
1. configure remmina connection
2. configure openconnect
3. place vpn_connect script in ~/bin
4. give script permissions: chmod 755 vpn_connect
5. add ~/bin to the environment variables' PATH
6. add the following to environment variables or replace in script:
    * $WORK_SERVER: server url
    * $WORK_GROUP: security group
    * $WORK_REMMINA_CONNECTION: path to connection file (most likely located in ~/.remmina)

### Usage
1. open terminal (Ctrl+Alt+t)
2. type vpn_connect and enter
    * this starts the vpn connection similar to a Cisco connection in Windows
3. open 2nd terminal (Ctrl+Alt+t)
4. type vpn_connect -o remmina
    * this establishes the remmina connection similar to a Remote Desktop connection in Windows
5. when done with connection, disconnect the remmina connection via the GUI
6. if the remmina connection terminal has not returned to the prompt, type Ctrl+c
7. in the vpn connection terminal window type Ctrl+c
8. finally, type vpn_connect -o disconnect to close the vpn connection

### Script
```bash
#!/bin/bash
# Connect to work vpn

function usage
{
    echo -e "usage:\n-c option:\n\t{blank: vpn connect,\
\n\t disconnect: vpn disconnect,\n\t remmina: remmina connect}\
\n-h"
}

# Main

option=

while [ "$1" != "" ]; do
    case $1 in
	-o | option )    shift
			  option=$1
			  ;;
	-h | --help )     usage
			  exit
			  ;;
	* )               usage
			  exit 1
    esac
    shift
done

if [ "$option" = "disconnect" ]; then
    # Disconnect from vpn
    sudo ifconfig tun1 down
    sudo openvpn --rmtun --dev tun1
elif [ "$option" = "remmina" ]; then
    # Make Remmina connection
    remmina -c $WORK_REMMINA_CONNECTION
else
    # Connect to vpn
    sudo openvpn --mktun --dev tun1
    sudo ifconfig tun1 up
    sudo openconnect $WORK_SERVER --authgroup=$WORK_GROUP --interface=tun1
fi

```
