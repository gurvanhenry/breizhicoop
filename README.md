# breizhicoop

POC with multiple apps in different docker containers in one server

## apps
- mattermost: http://breizhicoop-mattermost.gurvanhenry.com 
- nextcloud: http://breizhicoop-nextcloud.gurvanhenry.com
- espace membres: http://breizhicoop-membres.gurvanhenry.com
- static website: http://breizhicoop.gurvanhenry.com

## commands

### website (for static page and proxy)
- rebuild and run `docker rm -f website ; docker build -t website . ; docker run --name website -d -p 80:80 website`
- nginx applied conf `docker exec website nginx -T`
- error log `docker logs website`

### mattermost 
- create data folder: 
  - `mkdir -p ./volumes/app/mattermost/{data,logs,config,plugins}`
  - `chown -R 2000:2000 ./volumes/app/mattermost/`
- start container `docker-compose start`
- delete settings and data : `sudo rm -rf volumes`

### nextcloud
- `docker-compose up -d`

### elefan
- `docker-compose build`
- `docker-compose up -d`
- `docker-compose exec --user www-data gestion-compte composer install`
- `docker-compose exec gestion-compte php bin/console doctrine:schema:create`
- add "127.0.0.1 breizhicoop-membres.gurvanhenry.com" to your /etc/hosts
- visit "http://breizhicoop-membres.gurvanhenry.com/user/install_admin" to create the super admin user
- login with admin:password
- if needed: `docker-compose exec gestion-compte bash`

### other
- debug with `docker-compose logs -f`
- `docker-compose restart`

## Single-Sign-On

### mattermost
- OK on `http://breizhicoop.gurvanhenry.com:81`
- KO on `http://breizhicoop-mattermost.gurvanhenry.com`
- elefan act as a gitlab account 
- doc to help : https://docs.mattermost.com/deployment/sso-gitlab.html?highlight=gitlab
- config elefan:
  - create a service (see http://breizhicoop-membres.gurvanhenry.com/services/)
  - create a oauth client (see http://breizhicoop-membres.gurvanhenry.com/clients/)
- config mattermost:
  - edit `nano volumes/app/mattermost/config/config.json` 
  ```
    "GitLabSettings": {
        "Enable": true,
        "Secret": "5zkfnu55uu4gkc00k0gw4s0w0gw8wscw4sg4k4g4ow4kcw4g8g",
        "Id": "2_42cawzr8p5ic0oggcks4wwo8wgkgcsg8s0c0cg8os8gs8s8ckk",
        "Scope": "",
        "AuthEndpoint": "http://breizhicoop-membres.gurvanhenry.com/oauth/v2/auth",
        "TokenEndpoint": "http://breizhicoop-membres.gurvanhenry.com/oauth/v2/token",
        "UserApiEndpoint": "http://breizhicoop-membres.gurvanhenry.com/api/v4/user"
    }
    ```

## remarks
This is just a sandbox not ready to use in prod. (For instance, there are 2 nginx and 2 mariadb containers...)