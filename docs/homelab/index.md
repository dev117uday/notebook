# Hosting

## Rclone Mount

```bash
rclone mount b2-s3:udayy-vault /mnt/cloud-volume \
  --vfs-cache-mode full \
  --vfs-cache-max-size 30G \
  --vfs-write-back 30s \
  --dir-cache-time 24h \
  --allow-other \
  --daemon
```

```bash
sudo mkdir /mnt/cloud-volume
sudo chown -R $USER:$USER /mnt/cloud-volume
```

```bash
nohup rclone mount b2-s3:udayy-vault /mnt/cloud-volume   --vfs-cache-mode full   --vfs-cache-max-size 30G   --vfs-write-back 30s   --dir-cache-time 24h   --allow-other   --rc   --rc-web-gui   --rc-user admin   --rc-pass admin   --rc-addr :5572 > /dev/null 2>&1 &
```

## Systemd Service

```bash
sudo nano /etc/systemd/system/rclone-mount.service
```

```ini
[Unit]
Description=Rclone Mount & WebGUI for Immich
After=network-online.target

[Service]
Type=notify
User=udayy
Group=udayy
ExecStart=/usr/bin/rclone mount b2-s3:udayy-vault /mnt/cloud-volume \
  --vfs-cache-mode full \
  --vfs-cache-max-size 30G \
  --vfs-write-back 30s \
  --dir-cache-time 24h \
  --allow-other \
  --rc \
  --rc-web-gui \
  --rc-user admin \
  --rc-pass admin \
  --rc-addr :5572
ExecStop=/bin/fusermount -u /mnt/cloud-volume
Restart=always
RestartSec=10

[Install]
WantedBy=default.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl start rclone-mount
sudo systemctl enable rclone-mount
sudo systemctl start rclone-mount
sudo umount /mnt/cloud-volume
sudo killall rclone
```

```bash
rclone copy b2-s3:udayy-vault /mnt/cloud-volume \
  --transfers 32 \
  --checkers 64 \
  --s3-chunk-size 64M \
  --multi-thread-streams 8 \
  --fast-list \
  -P
```

```text
delete .cache/rclone/ folders
```

## Immich

```bash
./immich-go upload from-google-photos --concurrent-tasks=100  --on-errors=continue --server=http://10.1.1.122:2283 --api-key=W5GL4NH34bH7qR7WyfUzWL3l81IqGEl34O75ZHAfLrA /Users/udayy/Downloads/backup/takeout-*.zip
```

## Nextcloud

```bash
docker exec -it immich_postgres psql -U postgres -c "CREATE USER nextcloud WITH PASSWORD 'password';"
```

```bash
sudo docker exec -it immich_postgres psql -U postgres -c "CREATE DATABASE nextcloud OWNER postgres;"
```

```bash
mkdir -p /mnt/cloud-volume/nextcloud-data
```

```bash
sudo docker exec -it immich_postgres psql -U postgres -c "ALTER USER nextcloud WITH PASSWORD 'nextcloud_secure_pass';"
```

```bash
sudo chmod -R 0770 /mnt/cloud-volume/nextcloud-data/ && sudo chown -R 33:33 /mnt/cloud-volume/nextcloud-data/
```

```bash
echo "# Nextcloud data directory" > /mnt/cloud-volume/nextcloud-data/.ncdata
```

```bash
docker exec -it nextcloud sed -i "/);/i \ \ 'check_data_directory_permissions' => false," /var/www/html/config/config.php
```

```bash
docker compose restart nextcloud
```

## Docker Cleanup

```bash
sudo docker rmi -f $(sudo docker images -a -q)
sudo docker rm -vf $(sudo docker ps -a -q)
```

## Database Backup

```bash
docker exec -t immich_postgres pg_dumpall --clean --if-exists --username=postgres | gzip > ~/db-dump.sql.gz
```

```bash
gunzip < "/home/udayy/db-dump.sql.gz" \
  | sed "s/SELECT pg_catalog.set_config('search_path', '', false);/SELECT pg_catalog.set_config('search_path', 'public, pg_catalog', true);/g" \
  | sudo docker exec -i immich_postgres psql --username=postgres
```
