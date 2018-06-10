BRACELET-Lite
-----

Lite version of BRACELET, connecting scientific instruments running on old operating systems to [4CeeD](https://github.com/4ceed/4ceedframework/) cloud.

Based on the following open-source projects:

* [docker-elk](https://github.com/deviantony/docker-elk)
* [docker-bro](https://github.com/blacktop/docker-bro)
* [4ceeduploader](https://github.com/bracelet-project/4ceeduploader)

### Prerequisites
- [Docker](https://www.docker.com/community-edition#/download) version **1.10.0+**
- [Docker Compose](https://docs.docker.com/compose/install/) version **1.6.0+**
- A running instance of cloud-based [4CeeD](https://github.com/4ceed/4ceedframework/)
- A NGINX reverse proxy running on BRACELET edge server

### Configuration 

#### Configure 4CeeD Uploader
Configure `4ceeduploader` service in `docker-compose.yml` file with information of 4CeeD instance:
- `UPLOADER_HOME`: Home address of this uploader
- `CURATOR_HOME`: Alias address of the cloud-based 4CeeD instance that uploader will communicate with on behalf of the instruments. This address is set as `UPLOADER_HOME/curatorhome/`.
- `CURATOR_API_URL`: Alias address of the cloud-based 4CeeD APIs' base URL that uploader will communicate with on behalf of the instruments. This address is set as `UPLOADER_HOME/curatorapi/`.

#### Configure NGINX reverse proxy

Edit NGINX configuration file (i.e., often located at `/etc/nginx/nginx.conf`), and add the following reverse proxy configuration to the `http` server block:
```
location / {
    proxy_pass http://127.0.0.1:8000/;
}

location /curatorhome/ {
    proxy_pass http://[CURATOR_IP_ADDR];
}

location /curatorapi/ {
    proxy_pass http://[CURATOR_IP_ADDR]/api/;
}
```

In the above configuration, the first `location` block configures reverse proxy for accessing 4CeeD Uploader on BRACELET server, the second `location` block configures reverse proxy for accessing cloud-based 4CeeD instance via BRACELET, and the third `location` block configures reverse proxy for accessing APIs on cloud-based 4CeeD via BRACELET.

Run the following command for the new configurations to take effect:
```
sudo nginx -s reload
```

#### Configure Bro

Configure `bro` service in `docker-compose.yml` to listen to different network interface (default is `eth0`).

### Deploy

To run BRACELET-Lite:

```bash
docker-compose up
```

Or, to run BRACELET-Lite in detached mode:

```bash
docker-compose up -d
```

To stop BRACELET-Lite, simply press `Ctrl+C` if running in foreground mode, or run the following command if running in detached mode:

```bash
docker-compose down
```

After deploying BRACELET-Lite, 4CeeD uploader can be accessed at the address specify in `UPLOADER_HOME`.

### Next Steps

- [Setup Kibana](docs/setup_kibana.md) 
