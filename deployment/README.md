# Deploying MLApp

This directory contains manifests for the backend of the mlapp associated
with mlbot.net.

This is currently running on a GKE cluster.

See [machine-learning-apps/Issue-Label-Bot#57](https://github.com/machine-learning-apps/Issue-Label-Bot/issues/57) for a log of how
the service was deployed.

To build a new image

```
skaffold build
```

Then to update the image

```
cd overlays/dev|prod
kustomize edit set image gcr.io/github-probots/label-bot-frontend=gcr.io/github-probots/label-bot-frontend:${TAG}@${SHA}
```

## github-probots

There is a dedicated instance running in

* **GCP project**: github-probots
* **cluster**: kf-ci-ml
* **namespace**: mlapp

Deploying it

1. Create the deployment

   ```
   kubectl apply -f deployments.yaml  
   ```

1. Create the secret

   ```
   gsutil cp gs://github-probots_secrets/ml-app-inference-secret.yaml /tmp
   kubectl apply -f /tmp/ml-app-inference-secret.yaml
   ```

1. Create the ingress

   ```
   kubectl apply -f ingress.yaml
   ```


## issue-label-bot-dev

There is a staging cluster for testing running in

* **GCP project**: github-probots
* **cluster**:  kf-ci-ml
* **namespace**: label-bot-dev

Deploying it

1. Create the secrets



TODO(jlewi): instructions below are outdated

1. Create the deployment

   ```
   kubectl apply -f deployments-test.yaml  
   ```

1. Create the secret

   ```
   gsutil cp gs://github-probots_secrets/ml-app-inference-secret-test.yaml /tmp
   kubectl apply -f /tmp/ml-app-inference-secret-test.yaml -n mlapp
   ```

1. Create the service

   ```
   kubectl apply -f service-test.yaml
   ```

1. Create the ingress

   ```
   kubectl apply -f ingress-test.yaml
   ```
