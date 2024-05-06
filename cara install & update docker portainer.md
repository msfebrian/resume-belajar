# Install Portainer
## Pull Image latest
```
docker pull portainer/portainer-ce:latest
```

## jika belum ada volume portainer_data buat dahulu
```
docker volume create portainer_data
```

## run docker versi community edition.
```
docker run -d -p 8000:8000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

# Cara Update
## stop & hapus dahulu container portainer yg lama
```
docker stop portainer
docker rm portainer
```
## kemudian ikuti langkah seperti install
