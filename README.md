# Wordpress with modsecurity

## 1. Install

### 1.1. Wordpress

Based on [this](https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/).


1. Customize `setenv` and load the variables: `source setenv`
1. `cd wordpress`
1. Create namespace: `kubectl create ns $NAMESPACE`
1. Apply with kustomization: `envsubst < <(cat *.yaml) | kubectl apply -n $NAMESPACE -k -`
1. Finish setup by calling the URL of the Ingress and finish setting-up Wordpresss

### 1.2. ModSecurity

1. Lad the variables: `source setenv`
1. `cd waf`
1. Apply: `kubectl apply -n $NAMESPACE -f .`
1. Finish setup by calling the URL of the Ingress and finish setting-up Wordpresss

