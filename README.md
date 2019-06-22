# qBittorrent daemon(Web UI) running with Open VPN, IP tables included. Only your torrents will be hidden behind the VPN and you're free to browse the web at full speeds on your device.

**This originally came from [MarkusMcNugen/Docker-qBittorrentvpn](https://github.com/MarkusMcNugen/docker-qBittorrentvpn) 
It needed an update so I added more environment variables to make it more configurable.**

I didn't change much because much isn't needed. Just enough that it easier to use. **Health Check is inside as well, in case the torrent client** fails for some reason it can restart if you also use  [The Fixer](https://hub.docker.com/r/pmshydra/thefixer) and configure accordingly.

## Docker run command:
```
docker run --privileged  -d \
              -v /your/config/path/:/config \
              -v /your/downloads/path/:/downloads \
              -e "VPN_ENABLED=yes" \
              -e "OVPN_FILE=US_Chicago" \
              -e "LAN_NETWORK=192.168.0.0/24" \
              -e "NAME_SERVERS=1.1.1.1,1.0.0.1" \
              -e "VPN_USERNAME=VPN username" \
              -e "VPN_PASSWORD=VPN password" \
              -p 8080:8080 \
              -p 8999:8999 \
              -p 8999:8999/udp \
              pmshydra/qbvpn
```
### On first run it'll generate the config folder to where you configured it, place inside a .ovpn(or multiple) but make sure that the name is something like Country_Location.ovpn .
> Make sure if you don't enter the Username or Password to the VPN that you include it inside a credentials.conf using auth-user-pass credentials.conf along with your .ovpn file.
# Variables, Volumes, and Ports
## Environment Variables
| Variable | Required | Function | Example |
|----------|----------|----------|----------|
|`VPN_ENABLED`| Yes | Enable VPN? (yes/no) Default:yes|`VPN_ENABLED=yes`|
|`VPN_USERNAME`| No | If username and password provided, configures ovpn file automatically |`VPN_USERNAME=p1123456`|
|`VPN_PASSWORD`| No | If username and password provided, configures ovpn file automatically |`VPN_PASSWORD=0987655`|
|`OVPN_FILE`| Yes | If no file is entered, it will not know what to look for. |`OVPN_FILE=US_Chicago`|
|`LAN_NETWORK`| Yes | Local Network with CIDR notation |`LAN_NETWORK=192.168.0.0/24`|
|`NAME_SERVERS`| No | Comma delimited name servers |`NAME_SERVERS=1.1.1.1,1.0.0.1`|
|`PUID`| No | UID applied to config files and downloads |`PUID=1000`|
|`PGID`| No | GID applied to config files and downloads |`PGID=1000`|
|`UMASK`| No | GID applied to config files and downloads |`UMASK=002`|
|`WEBUI_PORT_ENV`| No | Applies WebUI port to qBittorrents config at boot (Must change exposed ports to match)  |`WEBUI_PORT_ENV=8080`|
|`INCOMING_PORT_ENV`| No | Applies Incoming port to qBittorrents config at boot (Must change exposed ports to match) |`INCOMING_PORT_ENV=8999`|

## Default username and password for webUI:
| Username | Password |
|----------|----------|
|`WebUI Username`| admin |
|`WebUI Password`| adminadmin |

###### HealthCheck works by checking if the webpage is normal at http://localhost:8080. If it is not accessible the container's status is marked as Unhealthy. Then if you are also using The Fixer, it'll automatically restart the container so your download's can resume like nothing happened.
