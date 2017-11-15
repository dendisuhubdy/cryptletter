<p align="center">
<img src="https://user-images.githubusercontent.com/1336156/31407710-267e2124-ae06-11e7-8a48-4e7dc3547422.png" />
<br>
</p>

## Introduction

Sending plaintext passwords unencrypted through the internet highway isn't just risky, it's ridiculous.
This project aims to make this process a bit more secure.

Usually an email inbox of a regular user contains more plaintext passwords than emails from rich african princes.

Retain control over the data which is send out, and prevent living-security-issues laying around in the users inbox.

## Demo

![Screen capture of the application](https://user-images.githubusercontent.com/1336156/32858885-31374bc8-ca4d-11e7-9b1a-b4a67769a241.gif)

## Features

- **Self-hosted**  
  Grab it. Use it.

- **Client side AES-256 encryption**  
  Messages are encrypted on the client side with the amazing [crypto-js](https://www.npmjs.com/package/crypto-js) library.

  The key is appended as a hash, so it never hits the server. (*In case of a hack on the micro-service, only encrypted garbage is captured*.)

- **Messages with a TTL**  
  Decide how long this message can survive after the client opens it.

- **Restricted message access**  
  Messages are restricted to the client's IP address at the first opening. Messages posted through facebook or other link checking tools prevent opening of the message.

---
## Quick usage
**Spoiler:** you have to setup a mysql database to get this setup running.

- Download the latest executable for your platform from [releases](https://github.com/Scribblerockerz/cryptletter/releases).
- Copy the `parameters.json` form the project and configure it properly.
- Run `./cryptletter-linux ./path/to/your/parameters.json &` to run it in the background

## Upgrade Guide (linux + wget)

Kill process and backup the current executable:
```bash

killall -9 cryptletter-linux

mv ./cryptletter-linux cryptletter-linux_old

```

Download the latest linux executable:

```bash

wget `wget -qO- https://api.github.com/repos/Scribblerockerz/cryptletter/releases/latest | grep -E -o 'https://[^"]+?cryptletter-linux'` && chmod +x ./cryptletter-linux

```

Run:

```bash

./cryptletter-linux parameters.json &

```

## Requirements
MySQL database.


## Changing Templates
All templates are rendered with [nunjucks](https://mozilla.github.io/nunjucks/). So they can be easily extended. Messages can be also changed this way.
<small>_You should change as less as possible to preserve update compatibility._</small>

###### 1. Create a theme
Create your `templates` directory and reference it in the `parameters.json`. Add an `assets` directory if you want to add custom css/images as well.
  ```json
  {
    "app": {
      "templates": "./child-views",
      "assets": "./custom-public-assets"
    }
  }
  ```
###### 2. Override existing once
Place templates with the same name to override it's parent. If you want to extend only some blocks, declare a `parent:` extend:
```nunjucks
{% extends 'parent:404.njk' %}

{% block words_not_found_message %}
    <h2>404</h2>
    <p>Message not found</p>
{% endblock %}
```

## Build

Build your own executable version with [pkg](https://www.npmjs.com/package/pkg).

Install `pkg` globally and run `npm run build`.
