# Astra Message Board demo app

Demo using Waline comment system to quickly create content and then backup/restore.

> This is for demo purposes only! Passwords are not properly stored and it is not optimized for security!

Simply deploy the yaml files on K8s (YAML folder). All container images are pulled from quay.io to avoid any issues with Docker Hub pull rate limits in lab environments. 
Get the loadbalancer IP and access the demo app via http://<IP>. Create some content, perform a backup. Then simulate a ransomware attack 
by going to http://<IP>/attack There is a login just to prevent accidential/premature attacks. Login with user _hack_ and password _astra123_. This will encrypt/scramble all comments. Refresh the demo app to see the result. Then restore.

## Components

* **nginx** - Reverse Proxy. Includes a Service that requests a loadbalancer. Frontend UI as well as the waline system will be available from this. Can be replaced with Ingress or similar but this should universally work. NGINX config is provided via ConfigMap, as is the htpasswd for authentication of the simulated ransomware attack. 
* **waline-frontend** Node.js + Epress. Hosts the static webpage (based on https://html5up.net/directive) and the simulated ransomware attack code
* **waline** The actual comment system (https://waline.js.org/). Uses a Postgres database to persist the comments
* **postgres** Simple Postgres database. SQL Code for initial database layout is provided via a Configmap. KISS principle for demo purposes, e.g. no Operator/HA,...

## Customization

Waline server can be configured via ENV variables, see docs at https://waline.js.org/en/reference/server/env.html

Waline layout can be modified via CSS, see docs at https://waline.js.org/en/reference/client/style.html

Avatar images are generated based on a hash of the nickname, using https://seccdn.libravatar.org/. Different styles can be used, try them at https://seccdn.libravatar.org/tools/check/ and also see docs at https://de.gravatar.com/site/implement/images/

Emojis are included from external source, can be changed in client config (index.js - Requires container rebuild). See https://www.jsdelivr.com/package/npm/@waline/emojis for options.

For Postgres configuration see https://hub.docker.com/_/postgres


