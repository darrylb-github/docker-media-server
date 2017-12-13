# docker-media-server

Docker compose example for running core media server software in docker.

Included:

- Plex - For playback and streaming.
- Sonarr - For managing TV shows.
- Radarr - For managing movies.
- Nzbget - For downloading via newsnet.

## Setup

- Make sure docker and docker-compose installed
- Clone this repo into your home folder
- Copy default env vars `cp .env.example .env`
- Symlink your movies, tvshows and downloads to the locations used in the docker-compose file:  
    - `sudo ln -s /home/myuser/tvshows /tvshows`
    - `sudo ln -s /home/myuser/movies /movies`
    - `sudo ln -s /home/myuser/downloads /downloads`
- Assign one user as owner for each of these dirs:
    - `sudo chown -R myuser:myuser /tvshows /movies /downloads`
- Run `id myuser` to get UID and GID of this user and update these in `.env`

## Usage
- `docker-compose up -d` - start all containers in daemon mode
- Run `docker-compose restart -d plex` whenever there are new plex versions available
- `docker-compose logs -f` will allow you to follow logs and see whats happening.
- `docker-compose exec sonarr bash` etc will give you a shell in the container.

## Configuration

For simplicity, this repo shares the same `.env` file for each container, since by default the environment variables are very similar.

If you need to apply additional environment variables for one container only, create a new `.env` file for that container and include it along with the base `.env` like so:

```
    env_file:
      - .env
      - sonarr.env
```

You could add the env vars directly to the docker-compose, but it's easier to backup your customisations if they are all in `.env` files.

## Backup & Restore
Back up `/opt/docker-media-server` to back up all of the config for plex, radarr, sonarr and nzbget.

Back up docker-compose.yml and/or `.env` files if you've customised any defaults.

Back up `/tvshows` and `/movies` to backup your media.

To restore from fresh, restore these directories from your backup then follow the setup and usage again as required.

Backup and restore scripts may be included in this repo in a future version.

## Remove

Sometimes you'll wan't to nuke everything, perhaps to do a fresh setup. See the following example and adjust as required:

```
cd docker-media-server
# Stop and remove all containers
docker-compose down --volumes
# Remove config dir that gets mapped into containers
rm -rf /opt/docker-media-server
# Remove repo files
cd ..
rm -rf docker-media-server
```
