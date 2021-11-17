## Docker Image For [BookStack](https://github.com/ssddanbrown/BookStack)


## Current Version: 21.11


### How to use the Image without Docker compose

Note that if you want to use LDAP, `$` has to be escape like `\$`, i.e. `-e "LDAP_USER_FILTER"="(&(uid=\${user}))"`

Networking changed in Docker v1.9, so you need to do one of the following steps.

#### Docker < v1.9

1. MySQL Container:

```bash
docker run -d \
-p 3306:3306 \
-e MYSQL_ROOT_PASSWORD=secret \
-e MYSQL_DATABASE=bookstack \
-e MYSQL_USER=bookstack \
-e MYSQL_PASSWORD=secret \
--name bookstack_db \
mysql:5.7.21
```

#### Docker 1.9+

1. Create a shared network:

```bash
docker network create bookstack_nw
```

2. Run MySQL container :

```bash
docker run -d --net bookstack_nw  \
-e MYSQL_ROOT_PASSWORD=secret \
-e MYSQL_DATABASE=bookstack \
-e MYSQL_USER=bookstack \
-e MYSQL_PASSWORD=secret \
 --name="bookstack_db" \
 mysql:5.7.21
```



#### Volumes
To access your `.env` file and important bookstack folders on your host system change `<HOST>` in the following line to your host directory and add it then to your run command:

```bash
--mount type=bind,source=<HOST>/.env,target=/var/www/bookstack/.env \
-v <HOST>:/var/www/bookstack/public/uploads \
-v <HOST>:/var/www/bookstack/storage/uploads
```
In case of a windows host machine the .env file has to be already created in the host directory otherwise a folder named .env will be created.

After these steps you can visit [http://localhost:8080](http://localhost:8080) . You can login with username 'admin@admin.com' and password 'password'.

### Inspiration

This is a fork of Kilhog and Solidnerd did the intial work, but I want to go in a different direction.
