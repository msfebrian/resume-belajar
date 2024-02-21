# Install PostgreSQL & pgadmin Docker
## Menjalankan Container PostgreSQL Docker
buat volume untuk PostgreSQL
```
docker volume create postgresql-data
```

Run PostgreSQL
```
docker run --name postgresql -e POSTGRES_USER=root -e POSTGRES_PASSWORD=isi_password -e POSTGRES_DB=namadb -v postgresql-data:/var/lib/postgresql/data -p 5432:5432 -d postgres
```

## Menjalankan pgadmin
download pgadmin
```
docker pull dpage/pgadmin4
```

buat volume untuk pgadmin
```
docker volume create pgadmin-data
```

Run container pgadmin non TLS
```
docker run -p 80:80 \
  -p 443:443 \
  -v pgadmin-data=/var/lib/pgadmin \
  -e 'PGADMIN_DEFAULT_EMAIL=user@domain.com' \
  -e 'PGADMIN_DEFAULT_PASSWORD=SuperSecret' \
  -d dpage/pgadmin4
```

Run container pgadmin TLS
```
docker run -p 443:443 \
  -v pgadmin-data:/var/lib/pgadmin \
  -v /path/to/certificate.cert:/certs/server.cert \
  -v /path/to/certificate.key:/certs/server.key \
  -v /tmp/servers.json:/pgadmin4/servers.json \
  -e 'PGADMIN_DEFAULT_EMAIL=user@domain.com' \
  -e 'PGADMIN_DEFAULT_PASSWORD=SuperSecret' \
  -e 'PGADMIN_ENABLE_TLS=True' \
  -d dpage/pgadmin4
```

## Masuk ke PostgreSQL di Terminal
```
psql -U nama_user --password --db nama_db
```

## Melihat list database
```
\l
```
## Melihat list relation
```
\dt
```

# Backup dan Restore
## Backup melalui docker
```
docker containerId/containerName pg_dump -U namaUser namaDatabase > \path\tujuan\backup\nama-backup.sql
```

## Restore ke docker
```
cat \path\lokasi\backup\nama-backup.sql | docker exec containerId/containerName psql -U namaUser namaDbDiPostgreSQL
```
