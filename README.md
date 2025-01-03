# Deploy Ollama onto Kubernetes

A POC on How-to deploy Ollama onto Kubernetes.

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

Run: `kubectl -n openweb-ui port-forward svc/openweb-ui-service 8080:8080`

Open: `http://localhost:8080`

Create a dummy first account.

## Gotchas

I've tried to use the [Ollama operator](https://github.com/nekomeowww/ollama-operator), but it seems to be partially implemented - which is fine, because it's on an early stage. Also, by design, it downloads all LLMs to a single StatefulSet, which might not be scalable. Ideally, you would want each pod to be independent rather than all the eggs in one basket.

So I've decided to go with the approach of breaking the manifests down to native Kubernetes manifests. It's also easier to configure it this way.

Because it's a local environment, I'm downloading the LLMs on pod startup to a shared directory inside each k3d container that serves as a "node". In production, a different method like a real NFS with periodical backups should be used.

## Troubleshooting

Check that the volume is created inside of the "kubernetes worker node".

```sh
docker exec -it k3d-k3s-default-agent-2 sh -c 'ls /nfs/ollama-models-store/'
```

Track that the model is being downloaded - depends on what model you're deploying, the download can take a while, if the pod dies, it will continue to download from where it stopped:

```sh
docker exec -it k3d-k3s-default-agent-2 sh -c 'watch du -sh /nfs/ollama-models-store/'
```
