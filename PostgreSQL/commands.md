# Find number of duplicated values
SELECT COUNT(*)
FROM (SELECT country, search_term, store_id, time, COUNT(*) as count
FROM position_installs
GROUP BY country, search_term, store_id, time
HAVING COUNT(*) > 1) as subq

# Addition of day and hour
parsed_at::date + interval '21 hours'

# Count distinct values for pair: country, search_term
SELECT COUNT(*)
FROM (SELECT DISTINCT country, search_term
    FROM result_data) as t1

# Show all processes
SELECT pid, age(clock_timestamp(), query_start), usename, query
FROM pg_stat_activity
WHERE query != '<IDLE>' AND query NOT ILIKE '%pg_stat_activity%'
ORDER BY query_start desc;

# Kill process
SELECT pg_cancel_backend(<pid>)

# Create remote database for MLflow
sudo adduser mlflow
sudo usermod -aG sudo mlflow
su - mlflow
sudo -u postgres psql
CREATE USER <postgres_user> PASSWORD <postgres_password>;
CREATE DATABASE <postgres_database>;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO <postgres_user>;
\q
sudo nano /etc/postgresql/12/main/pg_hba.conf
host all all 0.0.0.0/0 trust
sudo service postgresql restart
sudo nano /etc/postgresql/12/main/postgresql.conf
#listen_addresses = ‘localhost’ # what IP address(es) to listen on;
listen_addresses = ‘*’ # for Airflow connection
sudo service postgresql restart

# DELETE metadata (schema)
sudo -u postgres psql
\c <postgres_user>
DROP SCHEMA public CASCADE;
CREATE SCHEMA public;

GRANT ALL ON SCHEMA public TO postgres;
GRANT ALL ON SCHEMA public TO public;
