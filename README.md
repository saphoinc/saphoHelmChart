# Sapho

[Sapho](https://www.sapho.com/) is awesome.

## TL;DR;

```console
$ helm install sapho
```

## Introduction

This chart bootstraps a [Sapho](https://bitbucket.org/sapho/ops-docker-tomcat/) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

It also packages the [MySQL chart](https://github.com/kubernetes/charts/tree/master/stable/mysql) which is required for bootstrapping a MySQL deployment for the database requirements of the Sapho application.

## Prerequisites

- Kubernetes 1.4+ with Beta APIs enabled
- PV provisioner support in the underlying infrastructure

## Get this chart

Download the latest release of the chart from the [releases](../../../releases) page.

Alternatively, clone the repo if you wish to use the development snapshot:

```console
$ git clone https://github.com/kubernetes/charts.git
```

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm install --name my-release sapho
```

*Replace the `x.x.x` placeholder with the chart release version.*

The command deploys Sapho on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of the WordPress chart and their default values.

| Parameter                            | Description                              | Default                                                    |
| -------------------------------      | -------------------------------          | ---------------------------------------------------------- |
| `image`                              | Sapho image                              | `sapho/ops-docker-tomcat:{VERSION}`                              |
| `imagePullPolicy`                    | Image pull policy                        | `Always` if `image` tag is `latest`, else `IfNotPresent`   |
| `saphoDBuser`                        | User of the application                  | `user`                                                     |
| `saphoDBpass`                        | MySQL DB Password                        | `nil`                                                      |
| `saphoDBtype`                        | Sapho DB Type                            | `mysql`                                                    |
| `saphoDBhost`                        | Sapho DB HostName                        | `nil`                                                      |
| `mysql.mysqlRootPassword`            | MySQL admin password                     | `nil`                                                      |
| `serviceType`                        | Kubernetes Service type                  | `LoadBalancer`                                             |
| `persistence.enabled`                | Enable persistence using PVC             | `true`                                                     |
| `persistence.apache.storageClass`    | PVC Storage Class for Apache volume      | `generic`                                                  |
| `persistence.apache.accessMode`      | PVC Access Mode for Apache volume        | `ReadWriteOnce`                                            |
| `persistence.apache.size`            | PVC Storage Request for Apache volume    | `1Gi`                                                      |
| `persistence.wordpress.storageClass` | PVC Storage Class for WordPress volume   | `generic`                                                  |
| `persistence.wordpress.accessMode`   | PVC Access Mode for WordPress volume     | `ReadWriteOnce`                                            |
| `persistence.wordpress.size`         | PVC Storage Request for WordPress volume | `8Gi`                                                      |

The above parameters map to the env variables defined in [bitnami/wordpress](http://github.com/bitnami/bitnami-docker-wordpress). For more information please refer to the [bitnami/wordpress](http://github.com/bitnami/bitnami-docker-wordpress) image documentation.

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install --name my-release \
  --set wordpressUsername=admin,wordpressPassword=password,mariadb.mariadbRootPassword=secretpassword \
    stable/wordpress
```

The above command sets the WordPress application username and password to `admin` and `password` respectively. Additionally it sets the MariaDB `root` user password to `secretpassword`.

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml stable/wordpress
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Persistence

The [Bitnami WordPress](https://github.com/bitnami/bitnami-docker-wordpress) image stores the WordPress data and configurations at the `/bitnami/wordpress` and `/bitnami/apache` paths of the container.

Persistent Volume Claims are used to keep the data across deployments. This is known to work in GCE, AWS, and minikube.
See the [Configuration](#configuration) section to configure the PVC or to disable persistence.
