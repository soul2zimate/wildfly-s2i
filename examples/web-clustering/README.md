# Web session sharing between pod instances example

In this example we are provisioning a WildFly server and deploying a simple servlet.
Multiple replicas of the deployment are created, all sharing the same web sessions.

# WildFly Maven plugin configuration
High level view of the WildFly Maven plugin configuration

## Galleon feature-packs

* `org.wildfly:wildfly-galleon-pack`
* `org.wildfly.cloud:wildfly-cloud-galleon-pack`

## Galleon layers

* `cloud-server`
* `web-clustering`

## CLI scripts
WildFly CLI scripts executed at packaging time

* None

## Extra content
Extra content packaged inside the provisioned server

* None

# Openshift build and deployment
Technologies required to build and deploy this example

* Helm chart for WildFly `wildfly/wildfly`

# WildFly image API
Environment variables from the [WildFly image API](https://github.com/wildfly/wildfly-cekit-modules/blob/main/jboss/container/wildfly/run/api/module.yaml) that must be set in the OpenShift deployment environment

* None

# WildFly cloud feature-pack API
Environment variables defined by the cloud feature-pack used to configure the server

* `JGROUPS_PING_PROTOCOL`. Cluster members ping protocol. Automatically set by Helm charts to `dns.DNS_PING`
* `OPENSHIFT_DNS_PING_SERVICE_NAME`. OpenShift serverless ping service name.  Automatically set by Helm charts to `web-clustering-app-ping`. 
NB: the service is automatically generated by Helm charts.

# Pre-requisites

* You are logged into an OpenShift cluster and have `oc` command in your path

* You have installed Helm. Please refer to [Installing Helm page](https://helm.sh/docs/intro/install/) to install Helm in your environment

* You have installed the repository for the Helm charts for WildFly

 ```
helm repo add wildfly https://docs.wildfly.org/wildfly-charts/
```

# Example steps

1. Deploy the example application using WildFly Helm charts

```
helm install web-clustering-app -f helm.yaml wildfly/wildfly
```

2. Your replicas are now sharing the web session. You can access the application, note the Session ID then scale/un-scale replicas, the session ID is preserved.