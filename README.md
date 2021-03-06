# Build server

## Usage
### Start
`forever start --uid "thisuser" -a index.js`  
*(replace `thisuser` with the same user given in step 1 under Installation)*

### Stop
`forever stop index.js`

## Prerequisites
- node with npm
- nginx (with `include /var/www/vhosts/*;` in config)

### Nginx server for nodejs
```
upstream build-server {
  server 127.0.0.1:3000;
}

server {
  listen 80;
  server_name your.domain.here;

  location / {
    proxy_pass http://build-server;
  }
}
```

## Installation
1. Run `sudo visudo` and add the following line *(replace `thisuser` with an existing user)*:  
  `thisuser ALL=(ALL) NOPASSWD: /usr/sbin/service nginx start,/usr/sbin/service nginx stop,/usr/sbin/service nginx restart`
2. `yarn` or `npm install`
3. Copy and edit contents of `.env.example` to `.env` accordingly
4. [Create an access token](https://github.com/settings/tokens) and save the variable as `GITHUB_TOKEN` in `.env`
5. Add the webhook to your repo, making sure your `GIT_HOOK_SECRET` (in `.env`) is the same as the secret of your private repo.

### Contents of `.env`
| Variable          | Default (from `.env.example`) | Description                                                                             |
|-------------------|-------------------------------|-----------------------------------------------------------------------------------------|
| `NAME`            | Jekyll Build                  | Name of the server                                                                      |
| `GIT_HOOK_SECRET` | your-secret-here              | Git hook secret, make sure this is the same as the secret of your __private repo__         |
| `GITHUB_TOKEN`    | your-github-token             | [Create an access token](https://github.com/settings/tokens) and save the variable here |
| `BUILDER_DOMAIN`  | build.example.com             | The domain you want the buildserver to appear on                                        |

## Todo:
- nginx test
- do something with permissions, so repositories can only read/write in their own repo (for gulp/npm postinstall scripts etc)
