# Landing Page Helm Chart

Requirements: Helm version 3 or higher installed. More installation instructions can be found here: https://helm.sh/docs/intro/install/

This helm chart deploys four services:
- editor: Deployment hosting the editor app
- renderer: Deployment hosting the renderer app
- assets: Deployment hosting the assets app
- publisher: Cronjob running publisher commands

# Installation and Rollback

## Package the Helm Chart

In the base directory, run:
```
helm package landing-page
```
This will output a helm chart package named landing-page-0.1.0.tgz. We will be using this package to install the chart.

## Install and Upgrade

### To Install:
Please run:
```
helm install {release-name} landing-page-0.1.0.tgz -f {custom-values.yaml} --create-namespace -n {namespace}
```
- release-name: name of your release
- namespace: the namespace you want to install to
- custom-values.yaml: the values file with custom configs

This will install all the resources in the targetted namespace. You can view the installed helm chart with:
```
helm ls -n {namespace}
```

### To Upgrade
To upgrade the release, please run:
```
helm upgrade {release-name} -f {custom-values.yaml} -n {namespace}
```

## Rollback

Rolling back to the previous revision number:
```
helm rollback {release-name}
```
If you wish to rollback to a different revision, please run `helm history {release-name}` to find the release revision then run:
```
helm rollback {release-name} [REVISION]
```

# Key Values in values.yaml
| Field | Type | Description |
| --- | --- | --- |
| global.ingress.host |	string | Hostname used for the global ingress. |
| global.ingress.tls.enabled |	bool | Whether TLS is enabled for the ingress. (Default is false) |
| global.ingress.tls.existingSecretName | string |	Name of an existing TLS secret to use when TLS is enabled. |
| global.ingress.ingressClassName |	string | Ingress class name to use for the main ingress. |
| global.ingress.annotations | object | Additional annotations applied to the main ingress. |
| editor.replicaCount | int | Number of replicas for the editor service. |
| editor.autoscaling.enabled | bool | Whether autoscaling is enabled for the editor. (Default is false) |
| editor.image.repository | string | Container image repository for the editor. |
| editor.image.tag | string | Container image tag for the editor. |
| editor.logLevel | string | Log level for the editor component. |
| editor.resources.limits.cpu | string | CPU limit for the editor container. |
| editor.resources.limits.memory | string | Memory limit for the editor container. |
| editor.resources.requests.cpu | string | CPU resource request for the editor container. |
| editor.resources.requests.memory | string | Memory resource request for the editor container. |
| renderer.replicaCount | int | Number of replicas for the renderer service. |
| renderer.autoscaling.enabled | bool | Whether autoscaling is enabled for the renderer. (Default is false) |
| renderer.image.repository | string | Container image repository for the renderer. |
| renderer.image.tag | string | Container image tag for the renderer. |
| renderer.logLevel | string | Log level for the renderer component. |
| renderer.resources.limits.cpu | string | CPU limit for the renderer container. |
| renderer.resources.limits.memory | string | Memory limit for the renderer container. |
| renderer.resources.requests.cpu | string | CPU resource request for the renderer container. |
| renderer.resources.requests.memory | string | Memory resource request for the renderer container. |
| assets.replicaCount | int | Number of replicas for the assets service. |
| assets.image.repository | string | Container image repository for the assets component. |
| assets.image.tag | string | Container image tag for the assets component. |
| assets.logLevel | string | Log level for the assets component. |
| assets.resources.limits.cpu | string | CPU limit for the assets container. |
| assets.resources.limits.memory | string | Memory limit for the assets container. |
| assets.resources.requests.cpu | string | CPU resource request for the assets container. |
| assets.resources.requests.memory | string | Memory resource request for the assets container. |
| publisher.replicaCount | int | Number of replicas for the publisher service. |
| publisher.image.repository | string | Container image repository for the publisher component. |
| publisher.image.tag | string | Container image tag for the publisher component. |
| publisher.logLevel | string | Log level for the publisher component. |
| publisher.resources.limits.cpu | string | CPU limit for the publisher container. |
| publisher.resources.limits.memory | string | Memory limit for the publisher container. |
| publisher.resources.requests.cpu | string | CPU resource request for the publisher container. |
| publisher.resources.requests.memory | string | Memory resource request for the publisher container. |

# Ingress

The helm chart will deploy an ingress exposing the editor, renderer, and assets service at the hostname set in the values file.

| service | path |
| --- | --- |
| editor | / |
| renderer | /renderer |
| assets | /assets |

