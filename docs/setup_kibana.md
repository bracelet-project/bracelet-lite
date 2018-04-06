Setup Kibana
====

## Setup reverse proxy

Since Kibana is setup to expose to only localhost interface (as Kibana itself does not provide any authentication by default), to access Kibana, we first need to setup a reverse proxy on BRACELET-Lite server to bridge the access to Kibana.

We recommend to use (NGINX)[https://www.nginx.com/] as the reverse proxy for BRACELET-Lite. Please follow installation instructions on NGINX website.

## Setup basic authentication and access to Kibana

### Create new user for Kibana admin

First, verify that apache2-utils is installed.

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
    listen 5601;

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

Now, Kibana can be accessed via `[BRACELET_SERVER_ADDRESS]:5601` using username and password setup above.
