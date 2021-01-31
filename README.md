# SelfStreamLive-API
This repository contains all the API's used in SelfStream.Live system.

### Jitsi Server creation using Digital Ocean API V2 and Ansible.

- [x] Create a Basic Droplet with SSH key via API Digital Ocean V2.
- [x] Run Ansible Locally on Droplet creation using user-data.
- [ ] Create a Custom Jitsi Server with a SelfStream Subdomain passing as parameter.
- [ ] Pass token, RTMP url, Event-ID as Env Vars and Configure Jitsi to use it. 
- [ ] Jitsi Layout Custom (https://github.com/cketti/jitsi-hacks)

##### API Call to create Jitsi Custom Server

```

curl --request POST \
  --url https://api.digitalocean.com/v2/droplets \
  --header 'Authorization: Bearer 896ba0377c79ab6276fda8ffc423a66ded35480ab94be700fe2f53f05fb92db5' \
  --header 'Content-Type: application/json' \
  --cookie __cfduid=dd32467e27572b22a4c897e4c2e94906a1611618721 \
  --data '{
  "name": "meuevento-hosts.selfstream.live",
  "region": "nyc1",
  "size": "s-1vcpu-1gb",
  "image": "debian-10-x64",
  "ssh_keys": [
    29297645
  ],
  "backups": false,
  "ipv6": false,
  "user_data": " #cloud-config\nruncmd:\n - apt-get update\n - apt-get -y install sudo\n - sudo apt-get -y upgrade\n - sudo apt-get -y install git\n - sudo apt-get -y install gnupg\n -cd ~\n - git clone https://eb60ea0493dbc8963018d274eb6ef3856f69e774@github.com/guilhermeneves/SelfStreamLive-Jitsi.git\n - sed -i '\''$d'\'' /etc/apt/sources.list\n - echo \"deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main\" >> /etc/apt/sources.list\n - sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 93C4A3FD7BB9C367\n - sudo apt-get update\n - sudo apt-get -y install ansible"
}'

```
##### Creating/Removing a Record on Domain with Digital Ocean V2 API Requests

Creating ...

```

curl --request POST \
  --url https://api.digitalocean.com/v2/domains/selfstream.live/records \
  --header 'Authorization: Bearer 896ba0377c79ab6276fda8ffc423a66ded35480ab94be700fe2f53f05fb92db5' \
  --header 'Content-Type: application/json' \
  --cookie __cfduid=dd32467e27572b22a4c897e4c2e94906a1611618721 \
  --data '{
	"type": "A",
	"name": "meuevento-hosts",
	"data": "157.230.1.78",
	"priority": null,
	"port": null,
	"ttl": 30,
	"weight": null,
	"flags": null,
	"tag": null
}'

```

Removing ...

```
curl --request DELETE \
  --url https://api.digitalocean.com/v2/domains/selfstream.live/records/133119667 \
  --header 'Authorization: Bearer 896ba0377c79ab6276fda8ffc423a66ded35480ab94be700fe2f53f05fb92db5' \
  --header 'Content-Type: application/json' \
  --cookie __cfduid=dd32467e27572b22a4c897e4c2e94906a1611618721
```

##### Useful Digital Ocean V2 API Requests


List a Droplet Status, useful to get IP-Address, Status and created_at

```
curl --request GET \
  --url https://api.digitalocean.com/v2/droplets/228879284 \
  --header 'Authorization: Bearer 896ba0377c79ab6276fda8ffc423a66ded35480ab94be700fe2f53f05fb92db5' \
  --header 'Content-Type: application/json' \
  --cookie __cfduid=dd32467e27572b22a4c897e4c2e94906a1611618721

```

List Available Droplets Sizes

```
curl --request GET \
  --url https://api.digitalocean.com/v2/sizes \
  --header 'Authorization: Bearer 896ba0377c79ab6276fda8ffc423a66ded35480ab94be700fe2f53f05fb92db5' \
  --header 'Content-Type: application/json' \
  --cookie __cfduid=dd32467e27572b22a4c897e4c2e94906a1611618721
```

