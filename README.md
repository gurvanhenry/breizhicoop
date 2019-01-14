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

## remarks
This is just a sand and not ready to use in prod. (For instance, there are 2 nginx containers...)
