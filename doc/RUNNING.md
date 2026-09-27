# Running rAthena

## Mac OSX

### Database via Docker (MariaDB)

Instead of installing MariaDB/MySQL natively, you can run it in Docker:

```sh
docker run -d \
  --name rathena-db \
  -p 3306:3306 \
  -e MARIADB_ROOT_PASSWORD=your_secure_password \
  -e MARIADB_DATABASE=ragnarok \
  --restart unless-stopped \
  mariadb:10 \
  --character-set-server=utf8mb4 \
  --collation-server=utf8mb4_unicode_ci
```

Note: `--character-set-server` and `--collation-server` are arguments to the
`mariadbd` server process, not to `docker run` itself, so they must come
*after* the image name (`mariadb:10`). Placing them before it (mixed in with
the `docker run` options) will cause Docker to fail to start the container.

Replace `your_secure_password` with your own password, then point your
server's `conf/inter_athena.conf` (and `conf/import/inter_conf.txt` if used)
at `127.0.0.1:3306` with database `ragnarok` and the root user/password above.
