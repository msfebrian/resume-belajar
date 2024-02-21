# Install PostgreSQL & pgadmin Docker
## Menjalankan Container PostgreSQL Docker
```
docker run --name postgresql -e POSTGRES_USER=root -e POSTGRES_PASSWORD=isi_password -e POSTGRES_DB=namadb -v postgresql-data:/var/lib/postgresql/data -p 5432:5432 -d postgres
```

## Masuk ke PostgreSQL di Terminal
```
psql -U nama_user --password --db nama_db
```
