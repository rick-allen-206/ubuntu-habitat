# ubuntu-habitat

The ubuntu habitat project aims to create a simple deployment of a home media and utility server, providing services like:

* DNS and Ad protection using Adgaurd
* VPN using wiregaurd
* Media tools using Plex, Jacket, Sonarr, Radarr, and Ombi
* VPN protected torrent client using Transmission
* A dashboard for easy navigation of all internal tools using Heimdall
* Logging using InfluxDB, Graphana
* System Statistics using Glances
* Game server hosting using Pterodactyl
* Reverse proxy and front end encryption using Traefik

All of these services are reverse proxed by Traefik for a completely secure deployment using certs provided by LetsEncrypt

## Deployment

LetsEncrypt certs will automatically be created for most services

It's required to run the docker command for certbot before running docker-compose up if you want to want the front end to proxy ssh to the back end ptero containers. The commands can be found in the "certbot_runner" file.

Look through all the containers before running a docker-compose up to make sure you only have the containers you plan to use. Comment out, or remove the ones you don't plan on useing.

Create a .env file, useing the example.env file provided. You only need to provide the variables for containers you plan to use.

Create first pterodactyl user with:
`docker-compose exec ptero-panel php artisan p:user:make`