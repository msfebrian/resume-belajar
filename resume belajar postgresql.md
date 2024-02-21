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
buat volume untuk pgadmin
```
docker volume create pgadmin-data
```

```
docker pull dpage/pgadmin4
docker run -p 80:80 \
  -p 443:443 \
  -e 'PGADMIN_DEFAULT_EMAIL=user@domain.com' \
  -e 'PGADMIN_DEFAULT_PASSWORD=SuperSecret' \
  -v pgadmin-data=/var/lib/pgadmin \
  -d dpage/pgadmin4
```

## Masuk ke PostgreSQL di Terminal
```
psql -U nama_user --password --db nama_db
```
