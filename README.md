# g-flix
A self-hosted streaming service.

## Disclaimer
This open-source software is intended for personal use only and should not be used for illegal purposes. This software should not be used to infringe on copyright laws or distribute copyrighted material without the express permission of the copyright owner. The user assumes full responsibility for any legal issues that may arise from the use of this software for illegal purposes. The creators of this software disclaim any liability for such illegal usage and strongly recommend using it only for lawful and legitimate purposes.

## Environment Setup

### update

```sudo apt-get update --assume-yes && sudo apt-get upgrade --assume-yes```

### vpn
`sh <(curl -sSf https://downloads.nordcdn.com/apps/linux/install.sh)`

### editor
`sudo apt install code`

### docker
`curl -fsSL test.docker.com -o get-docker.sh && sh get-docker.sh`

`sudo usermod -aG docker ${USER}`

### python3 and pip3 (might not be necessary!)
`sudo apt-get install libffi-dev libssl-dev`

`sudo apt install python3-dev`

`sudo apt-get install -y python3 python3-pip`

### docker compose
`sudo pip3 install docker-compose`

### enable on system start
`sudo systemctl enable docker`

### create media and disk folders
`sudo mkdir -m a=rwx -p {/mnt/disk1,/mnt/media}`

### change external hdd mount (folder name might vary)
#### unmount
`lsblk`

`sudo umount /media/${USER}/PassportA`

#### re-mount
`df -hT`

`sudo mount /dev/sda1 /mnt/disk1`

### mergerfs
`sudo apt install mergerfs`

`sudo mergerfs -o defaults,allow_other,use_ino,cache.files=partial,dropcacheonclose=true,category.create=mfs,moveonenospc=true,minfreespace=1M /mnt/disk* /mnt/media`

### create `/mnt/media`
`mkdir -m a=rwx -p {/mnt/media/downloads,/mnt/media/tv,mnt/media/movies}`

### edit `etc/fstab`
`sudo blkid`

`code etc/fstab`

#### paste the two following lines into editor at end of the file
```
UUID=<uuid_from_blkid> <path_to_mount> <file_system_type> uid=1000,gid=1000,umask=0007,async,auto,rw 0 0
/mnt/disk* /mnt/media fuse.mergerfs defaults,allow_other,use_ino,cache.files=partial,dropcacheonclose=true,category.create=mfs,moveonenospc=true,minfreespace=1M 0 0
```

### pull down this repo to `~/g-flix`
`git clone https://github.com/gregjoeval/g-flix.git`

`git pull`

### create service config folders in `~/g-flix`
`mkdir -m a=rwx -p {/config/plex,/config/transmission,/config/prowlarr,/config/sonarr,/config/radarr,/config/overseerr}`

### create .env.secret & add values in `~/g-flix`
`code .env.secret`

### get Plex claim token
go to [plex.tv/claim](plex.tv/claim)

### start services
`docker compose up -d`

## Optional Setup

### Set Scheduled Reboot
This might help with memory leak issues. The system was experiencing i/o errors when downloading content and this avoids the problem.

`sudo crontab -e`

#### update, upgrade, and then reboot
`0 9 * * * sudo apt-get update --assume-yes && sudo apt-get upgrade --assume-yes && /sbin/shutdown -r now`

## Application Setup

### Prowlarr
- add indexers
- add download client
- add applications Sonarr & Radarr (need api keys)
- sync app indexers

### Plex
- create name
- add tv shows folder /tv
- add movies folder /movies
- enable remote access
- enable library scanning
- turn off transcoding if your CPU is not strong enough

### Sonarr & Radarr
- add root folder
- add download client
- add authentication

### Overseerr
- login with plex
- add plex
- add Sonarr & Radarr (need api keys)

## References

### Inspiration
- https://zacholland.net/a-complete-guide-to-setting-up-a-plex-home-media-server-with-automated-requests-downloads

### RPI docker and docker compose
- https://dev.to/elalemanyo/how-to-install-docker-and-docker-compose-on-raspberry-pi-1mo

### docker service static ip addresses
- https://stackoverflow.com/a/52184320

### docker image update
(has potential to wipe config if volumes mapped incorrectly or permissions are not set up correctly)
- https://stackoverflow.com/a/61362893

## deleting docker containers and images
- https://stackoverflow.com/a/44785784

### mounting drives
- https://www.tomshardware.com/how-to/mount-drives-linux

### use mergerfs to map external drives
- https://fedoramagazine.org/using-mergerfs-to-increase-your-virtual-storage/
- http://corywestropp.com/develop/articles/setting-up-mergerfs/
- https://www.linuxserver.io/blog/2016-02-02-the-perfect-media-server-2016

### setup fstab (mounting on startup)
- https://askubuntu.com/a/165462

### installing vpn cli
- https://support.nordvpn.com/FAQ/NordVPN-setup-tutorials/1047409772/How-to-configure-a-Raspberry-Pi.htm
- https://support.nordvpn.com/Connectivity/Linux/1325531132/Installing-and-using-NordVPN-on-Debian-Ubuntu-Raspberry-Pi-Elementary-OS-and-Linux-Mint.htm
