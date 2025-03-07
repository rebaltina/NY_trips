# NY_trips
This repository is an exercitation for docker

Downloading the data

```bash
wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2010-10.csv.gz 
```

Run container for postgres
```bash
docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v "$(pwd)"/NY_trips:/var/lib/postgresql/data \ ###ACHTUNG!!! IUSE $(pwd)!!!
  -p 5432:5432 \
  --network=pg-network \
  --name pg-database \
  postgres:13
```

Run container for pgAdmin
```bash
docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  --network=pg-network \
  --name pgadmin \
  dpage/pgadmin4
```

Build image
```bash
docker build -t taxi_ingest:v001 .
```

Run ingestion script
```bash
URL = "https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2019-10.csv.gz" # URL THAT WORKED
docker run -it \
  taxi_ingest:v001 \
    --user=root \
    --password=root \
    --host=localhost ###ACHTUNG!!! IT?S LOCALHOST!!!
    --port=5432 \
    --db=ny_taxi \
    --table_name=yellow_taxi_trips \
    --url=${URL}
```