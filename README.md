# k8s-prod
Using helm charts to manage an entire production environment, acting as a 'service manifest' by recording what software and their versions that are needed to spin up an end-to-end working environment. This way, there are no undocumented deployments and releases.

## Prerequisites
* kubectl create ns prod

## Install
* helm dependency update --skip-refresh
* helm install prod . --namespace prod
