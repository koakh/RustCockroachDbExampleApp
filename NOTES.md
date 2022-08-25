# NOTES

- [Build a Rust App with CockroachDB and the Rust-Postgres Driver | CockroachDB Docs](https://www.cockroachlabs.com/docs/stable/build-a-rust-app-with-cockroachdb.html)

follow notes **CockRoachDB - Start a Local Cluster (Secure) + DockerCompose (Secure)**

```shell
$ cd /srv/docker/oficial/cockroachdb/
$ docker-compose up -d


```shell
# init Cluster and Test connection
$ cockroach init --certs-dir=./cluster/node1/certs --host=localhost:26257
# connect to node1
$ cockroach sql --certs-dir=./cluster/node1/certs --host=localhost:26257
```

```sql
-- create user
CREATE USER IF NOT EXISTS maxroach;
-- some database
CREATE DATABASE bank;
-- grant
GRANT ALL ON DATABASE bank TO maxroach;
-- GRANT admin TO maxroach;
-- some data
CREATE TABLE bank.accounts (id INT PRIMARY KEY, balance DECIMAL);
INSERT INTO bank.accounts VALUES (1, 1000.50);
SELECT * FROM bank.accounts;

  id | balance
+----+---------+
   1 | 1000.50
(1 row)
```
```

now run app

message: "password authentication failed for user maxroach"
message: "user maxroach does not have INSERT privilege on relation accounts"

```shell
# inline query
$ cockroach sql --certs-dir=/srv/docker/oficial/cockroachdb/cluster/node1/certs -e 'SELECT id, balance FROM accounts' --database=bank
```

## Error cannot convert between the Rust type `i64` and the Postgres type `numeric`'

use `SMALLINT` and `i16`

"CREATE TABLE IF NOT EXISTS accounts (id SMALLINT PRIMARY KEY, balance SMALLINT)"

let id: i16 = row.get(0);
let balance: i16 = row.get(1);

type conversion getted from [A comprehensive mapping of PostgreSQL, Diesel, and Rust types](https://kotiri.com/2018/01/31/postgresql-diesel-rust-types.html)