# Wordpress with antivirus

This repo demonstrates multiple ways to scan uploads for viruses using clamav and:

- clamit
- modsecurity

running as a reverse proxy in front of wordpress to intercept multi-part mime uploads.

This method will not work if the app is not using multi-part but another javascript method instead (like e.g. Jira does).

## 1. Install

### 1.1. Wordpress

Based on [this](https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/).


1. Customize `setenv` and load the variables: `source setenv`
1. `cd wordpress`
1. Create namespace: `kubectl create ns $NAMESPACE`
1. Create secret: `kubectl -n $NAMESPACE create secret generic mysql-pass --from-literal=password="${PASSWORD}"`
1. Apply with substitution: `envsubst < <(cat wordpress-deployment.yaml) | kubectl -n $NAMESPACE apply -f -`
1. Finish setup by calling the URL of the Ingress and finish setting-up Wordpress

### 1.2 Clamav

1. Load the variables: `source setenv`
1. `cd clammit`
1. Apply clamav: `envsubst < <(cat docker-clamav-deployment.yaml) | kubectl -n $NAMESPACE apply -f -`

### 1.3. ModSecurity

1. Lad the variables: `source setenv`
1. `cd waf`
1. Apply: `kubectl apply -n $NAMESPACE -f .`
1. Change the `wordpress` ingress to point to `docker-waf` at port `8080` 

### 1.4. Clammit

1. Lad the variables: `source setenv`
1. `cd clammit`
1. Apply clammit: `envsubst < <(cat docker-clammit-deployment.yaml) | kubectl -n $NAMESPACE apply -f -`
1. Change the `wordpress` ingress to point to `docker-clammit` at port `8438` 

## 2. Test

Test files can be found here: https://www.eicar.org/download-anti-malware-testfile/


