# Setup tracking node in MAIN NW

```bash
$ cd sundoc
$ cp -rf .env.mainnw .env
$ cp -rf sunnycoind/etc/mainnw.tracking.example sunnycoind/etc/sunnycoind.cfg
$ cp -rf sunnycoind/etc/mainnw.validators.txt sunnycoind/etc/validators.txt

# Running sunnycoind
$ docker-compose up -d
```

# Stop & Restart & Rebuild
```bash
# Stop
docker-compose stop

# Restart
docker-compose restart

# Rebuild
docker-compose stop
docker-compose build --no-cache
```

# Export & Import

```bash
# Export
$ docker save sundock_sunnycoind:latest  > sundock_sunnycoind.tar

# Import
$ docker load < sundock_sunnycoind.tar
```
