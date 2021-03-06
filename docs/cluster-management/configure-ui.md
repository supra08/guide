---
layout: main
title: Configuring the UI
category: Cluster Management
menu: menu
toc: 
    - title: Managing the User Interface
      url: "#managing-the-user-interface"
      active: true
    - title: Packages
      url: "#packages"
    - title: Configuration
      url: "#configuration"
    - title: API Configuration
      url: "#api-configuration"
      subitem: true
    - title: Store Configuration
      url: "#store-configuration"
      subitem: true
    - title: Avatar Configuration
      url: "#avatar-configuration"
      subitem: true
    - title:  Custom Documentation Link
      url: "#custom-documentation-link"
      subitem: true
    - title:  Custom Slack Link
      url: "#custom-slack-link"
      subitem: true
    - title: Canary Routing
      url: "#canary-routing"
      subitem: true          
---
# Managing the User Interface

The User Interface has configuration options for the API, Data Store, Avatars, and Custom Documentation.

## Packages

Like the other services, the User Interface is shipped as a [Docker image](https://hub.docker.com/r/screwdrivercd/ui/) with port 80 exposed.

```bash
$ docker run -d -p 8000:80 screwdrivercd/ui:stable
$ open http://localhost:8000
```

Our images are tagged for their version (eg. `1.2.3`) as well as a floating `latest` and `stable`.  Most installations should be using `stable` or the fixed version tags.

## Configuration

The User Interface has a few configuration options, the location of the API, store, and path to avatar images.

### API Configuration
API hostname is set via an environment variable `ECOSYSTEM_API`.

Example:
```bash
$ docker run -d -p 8000:80 -e ECOSYSTEM_API=http://localhost:9000 screwdrivercd/ui:stable
```

### Store Configuration
Log and file store hostname is set via an environment variable `ECOSYSTEM_STORE`.

Example:
```bash
$ docker run -d -p 8000:80 -e ECOSYSTEM_STORE=http://localhost:9001 screwdrivercd/ui:stable
```

### Avatar Configuration
We've added content security protection headers to the docker image for the UI, so images loaded from external sources, such as avatars, must be configured in these headers. This is set via an environment variable `AVATAR_HOSTNAME`.

Example:
```bash
$ docker run -d -p 8000:80 -e AVATAR_HOSTNAME="avatars*.githubusercontent.com" screwdrivercd/ui:stable
```

Common examples for avatar hostnames are:
* Github: `avatars*.githubusercontent.com`
* Bitbucket: `bitbucket.org/account/*/avatar/*`
* GHE: `exampleghe.com/avatars/u/*`

Multiple hostnames can be added at once by separating them with a space.

Example:
```bash
$ docker run -d -p 8000:80 -e AVATAR_HOSTNAME="avatars*.githubusercontent.com bitbucket.org/account/*/avatar/*" screwdrivercd/ui:stable
```

### Custom Documentation Link
Documentation link can be customized via an environment variable `SDDOC_URL`.

Default: https://docs.screwdriver.cd

Example:
```bash
$ docker run -d -p 8000:80 -e SDDOC_URL=https://mydocs.mysite.me screwdrivercd/ui:stable
```

### Custom Slack Link
Slack instance link can be customized via an environment variable `SLACK_URL`.

Default: https://slack.screwdriver.cd/

Example:
```bash
$ docker run -d -p 8000:80 -e SLACK_URL=https://slack.mydomain.com screwdrivercd/ui:stable
```

### Canary Routing

If your Screwdriver Kubernetes Cluster is using [nginx Canary ingress](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/#canary), then set the following environment variables to have UI server set a cookie for a limited duration such that subsequent UI requests are served by same canary UI pods.

| Environment name     | Default Value | Description          |
|:---------------------|:--------------|:---------------------|
| CANARY_RELEASE | "" | Set to "true" to denote  that this UI server is serving Canary version of UI |
| RELEASE_VERSION | "stable" | UI Release version displayed under help menu in header|