# nextcloud-aio-cloudflare-tunnel
A Docker Compose container setup for [Nextcloud AIO](https://github.com/nextcloud/all-in-one) using [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) as the reverse proxy. This setup assumes Nextcloud AIO and the tunnel are on the same server.

This project is based on [Jonas Merkle](https://github.com/jonas-merkle) [container-cloudflare-tunnel](https://github.com/jonas-merkle/container-cloudflare-tunnel).

## Prerequisites
- Docker
- Cloudflare account with some configured domain

## Configuration
1. [Create a tunnel on Cloudflare](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/get-started/create-remote-tunnel/#1-create-a-tunnel). It's important to store the token in a secure place, we are going to use it in the next steps.
2. [Connect tunnel to Nextcloud AIO](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/get-started/create-remote-tunnel/#2-connect-an-application). Make sure to set the service with the value `http://nextcloud-aio-apache:11000`. We are using the name of the Apache container created by Nextcloud AIO because we will set up the tunnel and the app in the same network.
3. Add to `.env` file the variable `CLOUDFLARE_TUNNEL_TOKEN` with the token provided in the previous step. Make sure to not commit `.env` file to the remote repository, adding the file to `.gitignore` or just mark it as unchanged with the command below:
```shell
git update-index --assume-unchanged .env
```
4. Create the external network used by the app and the tunnel. If you are not using IPv6, you can remove `--ipv6` flag.
```shell
docker network create --ipv6 --driver bridge nextcloud-aio
```

## Usage
1. From the root of this repository's folder, run the following command:
```shell
docker compose up -d
```
2. Navigate to `https://localhost:8080` and follow the instructions to end the set up of Nextcloud AIO.

After a couple of minutes creating and starting all the containers, you should be able to access to your Nextcloud instance using the configured domain, for example `cloud.example.com` assuming you have configured this domain in the previous steps.
