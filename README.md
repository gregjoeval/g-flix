# g-flix

## Helpful Links
https://zacholland.net/a-complete-guide-to-setting-up-a-plex-home-media-server-with-automated-requests-downloads
https://dev.to/elalemanyo/how-to-install-docker-and-docker-compose-on-raspberry-pi-1mo
https://stackoverflow.com/a/52184320
https://support.nordvpn.com/FAQ/NordVPN-setup-tutorials/1047409772/How-to-configure-a-Raspberry-Pi.htm


mounting drives
https://www.tomshardware.com/how-to/mount-drives-linux

use mergerfs to map external drives
https://fedoramagazine.org/using-mergerfs-to-increase-your-virtual-storage/
http://corywestropp.com/develop/articles/setting-up-mergerfs/
https://www.linuxserver.io/blog/2016-02-02-the-perfect-media-server-2016


https://askubuntu.com/a/165462





---
sudo apt-get update && sudo apt-get upgrade

# editor
sudo apt install code

# docker
curl -fsSL test.docker.com -o get-docker.sh && sh get-docker.sh
sudo usermod -aG docker ${USER}

# python3 and pip3
sudo apt-get install libffi-dev libssl-dev
sudo apt install python3-dev
sudo apt-get install -y python3 python3-pip

# docker compose
sudo pip3 install docker-compose
# enable on system start
sudo systemctl enable docker

# change external hdd mount
lsblk
sudo umount /media/${USER}/PassportA

df -hT
sudo mount /dev/sda1 /mnt/disk1

# mergerfs
sudo apt install mergerfs
sudo mergerfs -o defaults,allow_other,use_ino,category.create=mfs,moveonenospc=true,minfreespace=1M /mnt/disk* /mnt/media

# create /mnt/media
mkdir -m a=rwx -p {/mnt/media/downloads,/mnt/media/tv,mnt/media/movies}

# pull down this repo to ~/g-flix
git pull https://github.com/gregjoeval/g-flix.git

# create service config folders in ~/g-flix
mkdir -m a=rwx -p {/config/plex,/config/transmission,/config/prowlarr,/config/sonarr,/config/radarr,/config/overseerr}

# create .env.secret & add values in ~/g-flix
touch .env.secret
# get claim token from plex.tv/claim

# start services
docker compose up -d
