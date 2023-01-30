# PART 03
# This is a continuation from [PART 02](PART_02.md)
---

# January 30, 2023
I came up with a basic `compose` file that makes the arcane `lima` error reproducible.
```bash
cat compose.yaml
version: '3'
services:
  test-cache:
    image: "redis:3.2.10"
    ports:
      - "6379:6379"
  test-fakesmtp:
    image: munkyboy/fakesmtp
    ports:
      - "1025:25"
    tty: true
  test-node:
    image: node:slim
    depends_on:
      - test-cache
      - test-fakesmtp
```

## On Linux
```bash
# ssh into a spare VM on AWS running Ubuntu

cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04 LTS"
```

Install `docker`:
```bash
sudo apt install docker.io
...
Need to get 69.2 MB of archives.
After this operation, 334 MB of additional disk space will be used.
Do you want to continue? [Y/n] y

docker --version
Docker version 20.10.12, build 20.10.12-0ubuntu2~20.04.1
```

Install `docker-compose`:
```bash
sudo apt install docker-compose
...
Need to get 262 kB of archives.
After this operation, 1616 kB of additional disk space will be used.
Do you want to continue? [Y/n] y


docker-compose --version
docker-compose version 1.25.0, build unknown
```

Ensure the daemon is running:
```bash
sudo service docker restart
```

Test the compose file:
```bash
cp compose.yaml /tmp
cd /tmp
sudo docker-compose -f compose.yaml up

Ctrl^C
```
Works as intended.


## On macOS
```bash
mkdir ~/tmp
cp compose.yaml ~/tmp/
cd ~/tmp
```

Test the compose file:
```bash
lima nerdctl compose up

INFO[0000] Creating network tmp_default
INFO[0000] Ensuring image munkyboy/fakesmtp
INFO[0000] Ensuring image redis:3.2.10
INFO[0000] Ensuring image node:slim
INFO[0000] Creating container tmp_test-cache_1
INFO[0000] Creating container tmp_test-fakesmtp_1
INFO[0000] Creating container tmp_test-node_1
FATA[0001] currently StdinOpen(-i) and Tty(-t) should be same
```

Terminate any running containers:
```bash
lima nerdctl compose down
```
