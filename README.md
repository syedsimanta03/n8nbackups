create n8n auto and manual backups to github repo:https://github.com/syedsimanta03/n8nbackups  for workflows and credentials. i'm using azure ubuntu docker n8n.

My linux cli and result below:

n8n@n8n:~$ cd ~/n8n
n8n@n8n:~/n8n$ ls
docker-compose.yml  n8n_data

n8n@n8n:~/n8n$ nano docker-compose.yml

: also my docker-compose.yml setup looks like this:

services:
  n8n:
    image: n8nio/n8n
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=12345678Sa@
      - WEBHOOK_URL=https://automation.designcoder.studio/
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_RUNNERS_ENABLED=true
      - N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true
    volumes:
      - ./n8n_data:/home/node/.n8n
      - ./n8n_backups:/home/node/backups

  git-sync:
    image: k8s.gcr.io/git-sync/git-sync:v3.6.3
    restart: always
    environment:
      - GIT_SYNC_REPO=https://syedsimanta03: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@github.com/syedsimanta03/n8nbackups.git
      - GIT_SYNC_BRANCH=main
      - GIT_SYNC_ROOT=/git
      - GIT_SYNC_ONE_TIME=false
      - GIT_SYNC_WAIT=60
    volumes:
      - ./n8n_backups:/git

git clone https://syedsimanta03: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@github.com/syedsimanta03/n8nbackups.git /home/node/backups
