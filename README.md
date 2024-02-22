# Tanka playground

This repository aim to play with tanka locally to familiarize with it.

# Requirements

- [`mise`](https://mise.jdx.dev/)
- [`direnv`](https://direnv.net/)

To setup dependencies you must run:

```sh
$ mise install
$ direnv allow
```

# Getting started

Those steps will help you setup a cluster with tanka configured in this folder.

## Create cluster

First you must create a kube cluster, we will use [`kind`](https://kind.sigs.k8s.io/):

```sh
$ kind create cluster --config ./kind/config.yaml
$ kind get kubeconfig --name local > .kubeconfig/default
```

Update [`tanka`](https://tanka.dev/) `apiServer` for
[`default environment`](/tanka/environments/default) in
[tanka/environments/default/spec.json#9](/tanka/environments/default/spec.json#9)
with the value return by the following command:

```sh
$ cat .kubeconfig/default | grep server | cut -d' ' -f6
https://127.0.0.1:44593
```

## Deploy with tanka

Move to [`tanka folder`](/tanka/) and install jsonnet dependencies:

```sh
$ cd tanka
$ jb install
```

Apply [`default environment`](/tanka/environments/default) to
[`kind`](https://kind.sigs.k8s.io/) cluster:

```sh
$ tk apply tanka/environments/default/
diff -u -N /tmp/LIVE-1182370706/apps.v1.Deployment.default.grafana /tmp/MERGED-257336423/apps.v1.Deployment.default.grafana
--- /tmp/LIVE-1182370706/apps.v1.Deployment.default.grafana     2024-02-22 16:56:06.905113732 +0100
+++ /tmp/MERGED-257336423/apps.v1.Deployment.default.grafana    2024-02-22 16:56:06.905113732 +0100
@@ -0,0 +1,43 @@
+apiVersion: apps/v1
+kind: Deployment
+metadata:
...
```

## Delete environment

```sh
$ kind delete clusters local
```

# License

[MIT](./LICENSE)
