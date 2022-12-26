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

* **Create a .env file, using the example.env file provided. You only need to provide the variables for containers you plan to use.**

* LetsEncrypt certs will automatically be created for most services. The only service that requires additional manual configuration steps is Pterodactyl. The commands for which can be found below.

* It's required to run the docker command for certbot before running docker-compose up if you want to want the front end to proxy ssh to the back end ptero containers. The commands can be found in a section bellow.

* Look through all the containers before running a docker-compose up to make sure you only have the containers you plan to use. Comment out, or remove the ones you don't want.

* If using Pterodactyl, you can create the first pterodactyl user with the following command: `docker-compose exec ptero-panel php artisan p:user:make`

## Lets Encrypt for Pterodactyl

Run the following command to set up let's encrypt for pterodactyl:

```dockerfile
sudo docker run -it --rm --name certbot \
  -v "/opt/ubuntu-habitat/components/data/certbot/letsencrypt/:/etc/letsencrypt/" \
  -v "/opt/ubuntu-habitat/secrets/cloudflare.ini:/tmp/cloudflare.ini" \
  certbot/dns-cloudflare certonly \
  --dns-cloudflare \
  --dns-cloudflare-credentials /tmp/cloudflare.ini \
  -d ptero-panel.example.com
```

## Env file varriable explanation

|Name|Descrription|
|---|---|
|`domain`|This is the domain name that all the subdmoains for the services crated by this dock-compose file will be created under.|
|`influxdb_admintoken`|The Influxdb admin token.|
|`influxdb_username`|The initial Influxdb user acount to create|
|`influxdb_password`|The initial Influxdb user password to create|
|`openvpn_local_network`|The address range to be considered "local" for openvpn. Traffic to/from the local address range will not go through the VPN tunnel.|
|`openvpn_config`|This is a special varriable used to configure how openvpn configures it's vpn tunnel. If you are using expressvpn for this container (which is the default) then this varriable should be set to the region you'd like to connect to. You can find a list of the openvpn config file [here](https://www.expressvpn.com/setup#manual) and you can ommit the .ovpn at the end of the profile you want to use. Further information about this container can be found [here](https://github.com/haugene/docker-transmission-openvpn).|
|`openvpn_username`|The username for your openvpn connection. This should match the username of the VPN service you're connecting to.|
|`openvpn_password`|The password for your openvpn connection. This should match the username of the VPN service you're connecting to.|
|`plex_claim`|This is the plex claim code for your plex server. See Plex's website for details on how to obtain this.|
|`ptero_database_root`|The root database password for the Ptero database.|
|`ptero_database_password`|The regual database password for the Ptero database.|
|`ptero_panel_mail_username`|The email username used to log into the SMTP server.|
|`ptero_panel_mail_password`|The email password used to log into the SMTP server.|
|`ptero_mail_from`|The email address Ptero will send email from.|
|`cf_api_email`|The email address used to sign into Cloudflare.|
|`cf_dns_api_token`|The API token used to sign into Cloudflare.|
|`diun_discord_webhook`|The discord webhook link for DIUN to post messages to discord with. See Discord's documention on how to obtain this.|
|`diun_discord_mentions`|The UID of the user or group to @mention in discord. See Discord's documention on how to obtain this.|
