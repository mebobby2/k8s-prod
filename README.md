# k8s-prod
Using helm charts to manage an entire production environment, acting as a 'service manifest' by recording what software and their versions that are needed to spin up an end-to-end working environment. This way, there are no undocumented deployments and releases.

## Prerequisites
* kubectl create ns prod

## Install
* helm dependency update
* helm install prod . --namespace prod
* for updates: helm upgrade prod . --namespace prod

## Uninstall
* helm delete prod --namespace prod

## Notes
While using a Helm chart to define the entire environment is interesting, there are some limitations; You cannot specify different K8s namespaces for subcharts. Hence you cannot segregate services/components that make up an e2e working environment into different logical namespaces. For example, your entire Prod environment is made up of three namespaces:
1. cicd
2. monitoring
3. core (where the application microservices are deployed to)

You cannot do this with Helm dependencies (subcharts) because you can't install subcharts to different K8s namespaces. A much better tool for this sort of thing is something like helmfile: https://github.com/roboll/helmfile
