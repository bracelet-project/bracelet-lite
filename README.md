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

### Configuration 
Configure `4ceeduploader` service in `docker-compose.yml` file with information of 4CeeD instance:
- `CURATOR_HOME`: Home address of 4CeeD curator instance
- `CURATOR_API_URL`: Address of 4CeeD curator APIs
- `UPLOADER_HOME`: Home address of this uploader

Configure `bro` service in `docker-compose.yml` to listen to different network interface (default is `eth0`).

### Usage 

To run BRACELET-Lite:

```bash
./docker-compose up
```

To run BRACELET-Lite in detached mode:

```bash
./docker-compose up -d
```

To stop BRACELET-Lite, simply press `Ctrl+C` if running in foreground mode, or run the following command if running in detached mode:

```bash
./docker-compose down
```
