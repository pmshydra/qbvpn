qBittorrent daemon(Web UI) running with Open VPN, IP tables included. Only your torrents will be hidden behind the VPN and you're free to browse the web at full speeds on your device. This originally came from https://github.com/MarkusMcNugen/docker-qBittorrentvpn . It needed an update so I added more environment variables to make it more configurable.

I didn't change much because much isn't needed. Just enough that it easier to use. Health Check is inside as well, in case the torrent client fails for some reason it can restart if you also use  https://hub.docker.com/r/pmshydra/thefixer and configure accordingly.

Docker run command:

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

On first run it'll generate the config folder to where you configured it, place inside a .ovpn(or multiple) but make sure that the name is something like Country_Location.ovpn .

Environment Variables:

PUID=1000
(User ID of config and download files)Note your mileage may vary, usually found by typing id into terminal.

PGID=130
(Group ID of config and download files)Note your mileage may vary, usually found by typing id into the terminal

VPN_USERNAME=Username #VPN Account username(PrivateInternetAccess, ExpressVPN, tunnelbear, etc.)

VPN_PASSWORD=Password #VPN Account Password

LAN_NETWORK=192.168.1.0/24 #This is the network all your local traffic is on, so lets say your local computer ip is 192.168.1.40, it would be 192.168.1.0/24. If it was 192.168.0.40 it would be 192.168.0.0/24.

Default username and password for webUI:

Can login at http://ip:8080 (http://192.168.1.40:8080,http://localhost:8080)

Username: admin

Password: adminadmin


HealthCheck works by checking if the webpage is normal at http://localhost:8080. If it is not accessible the container's status is marked as Unhealthy. Then if you are also using The Fixer, it'll automatically restart the container so your download's can resume like nothing happened.
