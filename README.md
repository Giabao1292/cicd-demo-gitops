# CI/CD demo GitOps

This repository contains the desired Kubernetes state, separate from the React
application source repository. Argo CD will later watch this repository and
reconcile the cluster to match it.

## Frontend

The frontend is declared in [apps/frontend](/apps/frontend). It runs the
immutable image `ghcr.io/giabao1292/cicd-demo-api:81505f5`, runs two replicas,
and is exposed on
Docker Desktop Kubernetes at `http://localhost:30080`.

For this first learning deployment, apply it manually:

```sh
kubectl apply -k apps/frontend
```

The next stage replaces that manual command with Argo CD sync.

## Argo CD

[argocd/frontend-application.yaml](/argocd/frontend-application.yaml) is the
bootstrap manifest. It instructs Argo CD to watch `apps/frontend` on `main`
and automatically sync it into the local `cicd-lab` namespace. Apply this
file once after installing Argo CD; every later app change is reconciled from
Git automatically.

`argocd/notifications-config.yaml` configures Argo CD to notify Slack after a
successful sync or a failed sync. It references a Kubernetes Secret rather
than storing the Slack webhook URL in Git.
cicd-demo-gitops
