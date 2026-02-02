## Octopus Argo CD Gateway

The helm chart is hosted on [Docker Hub](https://hub.docker.com/r/octopusdeploy/octopus-argocd-gateway-chart), where you can pull it using Helm.

## Versions

The Octopus Argo CD Gateway Helm chart follows [Semantic Versioning](https://semver.org/). Generally, version updates can be interpreted as follows:

- *major* - Breaking changes to the chart. This may include adding or removing of resources, breaking changes in the Octopus Argo CD Gateway application image or breaking changes to the structure of the `values.yaml`.
- *minor* - New non-breaking features. New features or improvements to the Octopus Argo CD Gateway application or helm chart itself.
- *patch* - Minor non-breaking bug fixes or changes that do not introduce new features.

----------------------------------------------

## Values

### Automatic Update

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| autoUpdate.enabled | bool | `true` | Indicates if the automatic update process CronJob is enabled. If set to false, the CronJob is not created  |
| autoUpdate.failedJobsHistoryLimit | int | `1` | The number of failed finished jobs to keep. Set to 0 to not keep any failed jobs. |
| autoUpdate.job.backoffLimit | int | `3` | Sets the number of times the job re-runs on failure |
| autoUpdate.schedule | string | `"0 0 * * *"` | A Cron expression for how often the CronJob executes.  |
| autoUpdate.serviceAccount | object | `{"annotations":{},"automountServiceAccountToken":true,"create":true,"name":""}` | Values for configuring the service account used by the automatic update Job |
| autoUpdate.serviceAccount.annotations | object | `{}` | Additional annotations for the service account |
| autoUpdate.serviceAccount.automountServiceAccountToken | bool | `true` | Controls if the service account token should be automatically mounted into the automatic update Job pod |
| autoUpdate.serviceAccount.create | bool | `true` | Specifies whether a service account should be created, if autoUpdate.enabled is set to false this will not be used |
| autoUpdate.serviceAccount.name | string | `""` | Name of an existing service account to use for the automatic update Job pod |
| autoUpdate.successfulJobsHistoryLimit | int | `1` | The number of successful finished jobs to keep. Set to 0 to not keep any successful jobs. |

### Gateway

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| gateway.argocd.authenticationToken | string | `""` | The bearer token used to authenticate with the Argo CD instance |
| gateway.argocd.authenticationTokenSecretKey | string | `"ARGOCD_AUTH_TOKEN"` | The key in the existing secret that contains the server access token. Only used when gateway.argocd.authenticationTokenSecretName is set. Defaults to ARGOCD_AUTH_TOKEN. |
| gateway.argocd.authenticationTokenSecretName | string | `""` | Name of an existing secret that contains and Argo CD authentication token |
| gateway.argocd.insecure | bool | `false` | Skip server certificate and domain verification on the TLS connection to the Argo CD instance |
| gateway.argocd.plaintext | bool | `false` | Disable TLS on the connection to the Argo CD instance |
| gateway.argocd.serverCertificate | string | `""` | The base64-encoded public key of the self-signed x509 certificate or root CA certificate used by the target Argo CD Server. Must be in the PEM format. |
| gateway.argocd.serverGrpcUrl | string | `""` | The gRPC url (including the port) of the Argo CD instance to communicate with |
| gateway.debug | bool | `false` | Enable debug logs |
| gateway.octopus.plaintext | bool | `false` | Disables TLS on the connection to the Octopus Deploy server This should only be used if your Octopus Server is running without a certificate on its gRPC listener. |
| gateway.octopus.serverCertificate | string | `""` | The base64-encoded public key of the self-signed x509 certificate or root CA certificate used by the target Octopus Server. Must be in the PEM format. |
| gateway.octopus.serverGrpcUrl | string | `""` | The gRPC url (including the port) of the Octopus Deploy server to communicate with |
| gateway.octopus.serverThumbprint | string | `""` | The thumbprint of the Octopus Deploy server the gateway is communicating with. This should only be used if you wish to pin the certificate. |
| gateway.serverCertificateSecretName | string | `""` | The name of a secret containing one or more base64-encoded public keys of x509 certificates used by Octopus and Argo CD that the Gateway should trust. The secret must be in the same namespace as the Gateway and all certificates must be in the PEM format. |
| gateway.serviceAccount | object | `{"annotations":{},"automountServiceAccountToken":true,"create":true,"name":""}` | Values for configuring the service account used by the gateway pod |
| gateway.serviceAccount.annotations | object | `{}` | Additional annotations for the service account |
| gateway.serviceAccount.automountServiceAccountToken | bool | `true` | Controls if the service account token should be automatically mounted into the gateway pod |
| gateway.serviceAccount.create | bool | `true` | Specifies whether a service account should be created |
| gateway.serviceAccount.name | string | `""` | Name of an existing service account to use for the gateway pod |

### Image

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| image.imagePullSecrets | list | `[]` | This is for the secrets for pulling an image from a private repository more information can be found here: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/ |
| image.pullPolicy | string | `"IfNotPresent"` | Image pull policy |
| image.registry | string | `"docker.io"` | Registry host to pull images from |
| image.repository | string | `"octopusdeploy/octopus-argocd-gateway"` | Image name to use |
| image.tag | string | `.Chart.AppVersion` | Image tag to use |
| image.tagSuffix | string | `""` | Suffix to append to the image tag |

### Registration

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| registration.argocd.webUiUrl | string | `""` | The URL of the Argo CD instance's Web UI |
| registration.octopus.environments | list | `[]` | Environment slugs or ids that the gateway should be associated with |
| registration.octopus.name | string | `""` | Name of the gateway |
| registration.octopus.serverAccessToken | string | `""` | The access token to authenticate to Octopus Deploy to register with |
| registration.octopus.serverApiUrl | string | `""` | The API URL of Octopus Deploy for registration e.g. https://my-instance.octopus.app |
| registration.octopus.serverCertificate | string | `""` | The base64-encoded public key of the self-signed x509 certificate or root CA certificate used by the Octopus Server for its HTTP API. Must be in the PEM format. |
| registration.octopus.spaceId | string | `""` | The space id that the gateway is registering with |
| registration.register | bool | `true` | Automatically register the gateway with the Octopus Deploy server, if set to false the gateway will not register itself |
| registration.serviceAccount | object | `{"annotations":{},"automountServiceAccountToken":true,"create":true,"name":""}` | Values for configuring the service account used by the registration pod |
| registration.serviceAccount.annotations | object | `{}` | Additional annotations for the service account |
| registration.serviceAccount.automountServiceAccountToken | bool | `true` | Controls if the service account token should be automatically mounted into the registration pod |
| registration.serviceAccount.create | bool | `true` | Specifies whether a service account should be created, if registration.register is set to false this will not be used |
| registration.serviceAccount.name | string | `""` | Name of an existing service account to use for the registration pod |

### Other Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Custom affinity for kubernetes gateway pods |
| nameOverride | string | `""` | This is to override the chart name. |
| nodeSelector | object | `{}` | Custom node selector for kubernetes gateway pods |
| podAnnotations | object | `{}` | Annotations to be added to kubernetes gateway pods |
| podLabels | object | `{}` | Labels to be added to kubernetes gateway pods |
| podSecurityContext | object | `{}` | Security context for kubernetes gateway pods |
| resources | object | `{}` | Custom resources for kubernetes gateway pods |
| securityContext | object | `{}` | Security context for kubernetes gateway containers |
| tolerations | list | `[]` | Custom tolerations for kubernetes gateway pods |

----------------------------------------------

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| Octopus Deploy | <support@octopus.com> | <https://octopus.com> |

Autogenerated from chart metadata using [helm-docs](https://github.com/norwoodj/helm-docs)