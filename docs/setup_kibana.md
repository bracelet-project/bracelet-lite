Setup Kibana
====

## Setup reverse proxy

Since Kibana is setup to expose to only localhost interface (as Kibana itself does not provide any authentication by default), to access Kibana, we first need to setup a reverse proxy on BRACELET-Lite server to bridge the access to Kibana.

We recommend to use [NGINX](https://www.nginx.com/) as the reverse proxy for BRACELET-Lite. Only Ubuntu, you can install NGINX by issuing the following command:

```
sudo apt-get install nginx 
```

Please follow installation instructions on NGINX website.

## Setup basic authentication and access to Kibana

### Create new user for Kibana admin

First, verify that apache2-utils is installed, or issuing the following command to install it:

```
sudo apt-get install apache2-utils 
```

To create a password file and a first user, run the `htpasswd` utility with the `-c` flag (to create a new file), the file pathname as the first argument, and the username as the second argument:

```
sudo htpasswd -c /etc/nginx/htpasswd.users kibanaadmin
```

Press Enter and type the password for `kibanaadmin` at the prompts.

### Configure NGINX to allow access to Kibana 

Next, open NGINX's configuration file:

```
sudo vi /etc/nginx/sites-available/default
```

Add the following `server` block to NGINX's configuration file to allow access to Kibana that requires basic user/password authentication setup above:

```
server {
    listen 8080;

    server_name [BRACELET_SERVER_ADDRESS];

    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/htpasswd.users;

    location / {
        proxy_pass http://localhost:5601;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

After that, reload NGINX configuration:

```
sudo nginx -s reload
```

Now, Kibana can be accessed via `[BRACELET_SERVER_ADDRESS]:8080` using username and password setup above.

## Configure Kibana on the first access

After having access to Kibana, we need to configure Kibana to access Logstash data that has been indexed into Elasticsearch.

On Kibana's interface, click the *Management* tab, then the *Index Patterns* tab. Click *Add New* to define a new index pattern. Specify `logstash-*` as the index pattern for the Logstash data and click *Create* to define the index pattern.

The Logstash data set does contain time-series data, so after clicking *Add New* to define the index for this data, make sure the Index contains time-based events box is checked and select the @timestamp field from the Time-field name drop-down.

After configuring Kibana's index pattern for Logstash data, navigate to *Discover* tab to see visualization of real-time network traffic.
