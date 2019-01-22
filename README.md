## breizhicoop

POC with multiple apps in different docker containers in one server

### apps
- mattermost: http://breizhicoop-mattermost.gurvanhenry.com 
- nextcloud: http://breizhicoop-nextcloud.gurvanhenry.com
- static website: http://breizhicoop.gurvanhenry.com

### commands

#### website (for static page and proxy)
- rebuild and run `docker rm -f website ; docker build -t website . ; docker run --name website -d -p 80:80 website`
- nginx applied conf `docker exec website nginx -T`
- error log `docker logs website`

#### mattermost / nextcloud
- `docker-compose up -d`

#### elefan
- `docker-compose build`
- `docker-compose up -d`
- `docker-compose exec --user www-data gestion-compte composer install`
- `docker-compose exec gestion-compte php bin/console doctrine:schema:create`
- add "127.0.0.1 breizhicoop-membres.gurvanhenry.com" to your /etc/hosts
- visit "http://breizhicoop-membres.gurvanhenry.com/user/install_admin" to create the super admin user
- login on "http://breizhicoop-membres.gurvanhenry.com" with (admin:password)
- if needed: `docker-compose exec gestion-compte bash`

## remarks
This is just a sandbox not ready to use in prod. (For instance, there are 2 nginx and 2 mariadb containers...)
