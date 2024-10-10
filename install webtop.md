# Install Linux Desktop Environment via Docker dengan Webtop
## versi webtop debian kde arm64v8
1. docker pull
```
docker pull linuxserver/webtop:arm64v8-debian-kde
```
2. Buat folder untuk docker volume
contoh path
```
/DATA/AppData/webtop-debian-kde
```
3. sesuaikan path volume, port, password dan lain
```
services:
  webtop:
    image: linuxserver/webtop:arm64v8-debian-kde
    container_name: webtop-debian-kde

    security_opt:
      - seccomp:unconfined #optional
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Jakarta
      - SUBFOLDER=/ #optional
      - TITLE=Webtop #optional
      - CUSTOM_USER=isi_username
      - PASSWORD=isi_password
    volumes:
      - /DATA/AppData/webtop-debian-kde:/config
      - /var/run/docker.sock:/var/run/docker.sock #optional
    ports:
      - 4000:3000
      - 4001:3001
    devices:
      - /dev/dri:/dev/dri #optional
    shm_size: "2gb" #optional
    restart: unless-stopped
```
