# Deploy OpenWeb-UI onto Kubernetes

A simple way to test OpenWeb-UI features locally.

## Prerequisites

- [flox](https://flox.dev/docs/install-flox/)
- [docker](https://docs.docker.com/get-docker/)

## Quick Start

1. Activate the flox environment:

```sh
flox activate
```

2. Create the Kubernetes cluster:

```sh
task cluster-create
```

3. Deploy:

```sh
task deploy
```

## UI

Run: `kubectl -n openweb-ui port-forward svc/openweb-ui-service 8080:8080` or `task ui`.

Open: `http://localhost:8080`

Create a dummy first account.
