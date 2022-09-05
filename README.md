## OpenFaaS Pro Checker

This is a diagnostic tool for OpenFaaS customers when working with our support team.

## How it works

You deploy a one-time Kubernetes job that runs some queries against the Kubernetes API using a limited RBAC definition. The results are printed to the pod's logs and it's completely offline. There is no call-home or data transferred to any third-party.

You must email the results to us, or attach them to a Slack conversation.

## What's collected

OpenFaaS core components:

* Whether you're using OpenFaaS Pro
* Which image tags you're using
* Timeout values

Functions:

* Function names
* Auto-scaling settings
* The number of replicas for each function

## What's not collected

Confidential data, secrets, other environment variables.

## Run the job and collect the results

1) Deploy the job:

```bash
#!/bin/bash

kubectl apply -f ./artifacts/
```

2) Collect the results from the logs:

```bash
#!/bin/bash

JOBUUID=$(kubectl get job -n openfaas $JOBNAME -o "jsonpath={.metadata.labels.controller-uid}")
PODNAME=$(kubectl get po -n openfaas -l controller-uid=$JOBUUID -o name)

kubectl logs -n openfaas $PODNAME > $(date '+%Y-%m-%d_%H_%M_%S').txt
```

Remove the job and its RBAC permissions:

```bash
kubectl delete -f ./artifacts

```
