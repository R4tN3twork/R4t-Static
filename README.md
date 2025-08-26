# R4t
### A proxy site

<a href="https://repl.it/github/binary-person/womginx"><img src="https://docs.replit.com/images/repls/run-on-replit.svg" alt="Replit" width="150"/></a>


[![Deploy to Render](https://binbashbanana.github.io/deploy-buttons/buttons/remade/render.svg)](https://render.com/deploy?repo=https://github.com/R4tN3twork/R4t)
[![Deploy to Heroku](https://binbashbanana.github.io/deploy-buttons/buttons/remade/heroku.svg)](https://heroku.com/deploy/?template=https://github.com/R4tN3twork/R4t)
## Site compatibility

What works:
1. recaptcha
2. discord with actual login credentials (no qr code required)
3. websocket sites
4. sites that have cookies

What doesn't work:
1. react sites
2. sites that depend on window.location and are minified
3. YouTube UI (however, you can watch a video like https://proxysite.com/main/https://www.youtube.com/watch?v=vidid and it will work)

## Prerequisites

You need one thing and one thing only: nginx.

Ok maybe not just that, you'll need the following:
1. nginx
2. certbot
3. nodejs (for building wombat)
4. a (sub)domain
5. a vps

## Installation (more simple and contained)

1. [Install Docker](https://docs.docker.com/engine/install/ubuntu/)
2. [Install docker-compose](https://docs.docker.com/compose/install/)
3. Clone this repo by running `git clone https://github.com/binary-person/womginx`
4. `cd womginx` then edit go and edit `docker-compose.yml`
    - To disable safe browsing, delete the line that says `SAFE_BROWSING`
    - To change the port, edit `80:80` to `newport:80`
    - Don't pay attention to `PORT=80` as changing that only changes the port inside the docker container. Only weird environments like Heroku need it.
    - To bind the port locally, do `127.0.0.1:80:80` (do this if you're hosting multiple things and you're using a reverse proxy like nginx or caddy)
    - If you are using a reverse proxy, set the `x-forwarded-for` as womginx's rate limiter relies on this header when running in a container. So for nginx, add `proxy_set_header X-Forwarded-For $remote_addr;`
5. then run `sudo docker-compose up -d` to start it
6. `sudo docker-compose down` to stop
7. If you want to update womginx to the latest version, run `git pull && sudo docker-compose up -d --build`

## Installation (more complex, direct, and permanent)

```bash
# assuming you are on Ubuntu/Debian

## Part 1 of installation ##

# update your repo list
sudo apt update

# 1. install nginx
sudo apt install nginx

# 2. install certbot and the nginx plugin
sudo apt install certbot python3-certbot-nginx

# 3. install nodejs (skip if you already have it)
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install nodejs

# 4. Get a (sub)domain with an A(AAA) record pointing to your VPS IP

# 5. Get an ssl certificate
sudo certbot --nginx certonly -n -d yourdomain.com

## Part 2 of installation ##

# 1. clone repo and wombat submodule
git clone --recursive https://github.com/binary-person/womginx

# 2. build wombat
cd womginx/public/wombat
npm install
npm run build-prod

# 3. replace 'womginx.arph.org' with 'yourdomain.com' in nginx.conf
cd .. # cd into public folder
sed -i -e 's/womginx.arph.org/yourdomain.com/g' ../nginx.conf

# 4. replace '/home/binary/womginx/public' with your public folder
sed -i -e "s/\/home\/binary\/womginx\/public/$(pwd | sed -e 's/\//\\\//g')/g" ../nginx.conf

# 5. make backup of original nginx.conf
sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup

# 6. copy womginx nginx.conf to /etc/nginx/nginx.conf
sudo cp ../nginx.conf /etc/nginx/nginx.conf

# 7. restart the nginx server
sudo service nginx restart
```
## License

This project is licensed under the AGPL license.

Wombat uses AGPL, and under the terms of that license, this project, therefore, is also required to be licensed under AGPL.

## Credits

- [Binary-person](https://github.com/binary-person)-
  Created womginx
- [Jonas](https://github.com/jonasrhee1208wastaken)-
  Created an about:blank cloak
