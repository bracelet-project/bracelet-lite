BRACELET-Lite
-----

Lite version of BRACELET, connecting scientific instruments running on old operating systems to [4CeeD](https://github.com/4ceed/4ceedframework/) cloud.

Based on the following Docker images:

* [docker-elk](https://github.com/deviantony/docker-elk)
* [docker-bro](https://github.com/blacktop/docker-bro)
* [4ceeduploader](https://github.com/bracelet-project/4ceeduploader)

### Configuration 
Configure the `4ceeduploader` service in `docker-compose.yml` file with information of 4CeeD instance:
- `CURATOR_HOME`: Home address of 4CeeD curator instance
- `CURATOR_API_URL`: Address of 4CeeD curator APIs
- `UPLOADER_HOME`: Home address of this uploader

### Usage 

To run BRACELET-Lite:

```bash
./docker-compose up
```

