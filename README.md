# Sample Rabbitmq App

This is a sample of a Java Spring app that works with the Tanzu Application Platform and Rabbitmq.

## Install

Easiest way is to use kapp and ytt:

```
kapp deploy -a $(basename $PWD) -f <(ytt -f workload.yaml -f values.yaml -v domain=${CHANGEME_DOMAIN}) -n apps -c
```

**NOTE** Currently refers to `sample-rabbitmq-app-registration-manual` in the service binding, this is taking the App SSO secret and modifying it so that it works with service binding:

```
apiVersion: v1
stringData:
  # See more here: https://github.com/spring-cloud/spring-cloud-bindings#spring-security-oauth2
  type: oauth2 # Added, must always be this value "oauth2".
  client-id: # changed from clientId
  client-secret: # changed from clientSecret
  issuer-uri: # changed from issuerURI
  provider: # Added, this corresponds to spring.security.oauth2.client.registration.{name}.provider
kind: Secret
metadata:
  name: sample-rabbitmq-app-registration-manual
  namespace: apps
type: Opaque
```

## Dependencies

1. [kubectl CLI](https://kubernetes.io/docs/tasks/tools/)
1. Tanzu CLI and the apps plugin v0.2.0 which are provided as part of [Tanzu Application Platform](https://network.tanzu.vmware.com/products/tanzu-application-platform)
1. A cluster with the [Rabbitmq cluster operator](https://github.com/rabbitmq/cluster-operator) installed.
1. A cluster with Tanzu Application Platform, and the "Default Supply Chain" and Services Toolkit components installed, plus their dependencies. These components are a part of [Tanzu Application Platform](https://network.tanzu.vmware.com/products/tanzu-application-platform).

## Usage after deployment

Once deployed, you can `curl` the top-level domain of the app and receive a response.

## Maintainers

Glen - @gmrodgers
Konstantin - @jhvhs
Sam - @Samze