# BaGet :baguette_bread:

[![Build Status](https://sharml.visualstudio.com/BaGet/_apis/build/status/loic-sharma.BaGet)](https://sharml.visualstudio.com/BaGet/_build/latest?definitionId=2) [![Join the chat at https://gitter.im/BaGetServer/community](https://badges.gitter.im/BaGetServer/community.svg)](https://gitter.im/BaGetServer/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

A lightweight [NuGet](https://docs.microsoft.com/en-us/nuget/what-is-nuget) and [Symbol](https://docs.microsoft.com/en-us/windows/desktop/debug/symbol-servers-and-symbol-stores) server.

<p align="center">
  <img width="100%" src="https://user-images.githubusercontent.com/737941/50140219-d8409700-0258-11e9-94c9-dad24d2b48bb.png">
</p>

## Getting Started

1. Install [.NET Core SDK](https://www.microsoft.com/net/download)
2. Download and extract [BaGet's latest release](https://github.com/loic-sharma/BaGet/releases)
3. Start the service with `dotnet BaGet.dll`
4. Browse `http://localhost:5000/` in your browser

For more information, please refer to [our documentation](https://loic-sharma.github.io/BaGet/).

## Features

* Cross-platform
* [Dockerized](https://loic-sharma.github.io/BaGet/#running-baget-on-docker)
* [Cloud ready](https://loic-sharma.github.io/BaGet/cloud/azure/)
* [Supports read-through caching](https://loic-sharma.github.io/BaGet/configuration/#enabling-read-through-caching)
* Can index the entirety of nuget.org. See [this documentation](https://loic-sharma.github.io/BaGet/tools/mirroring/)
* Coming soon: Supports [private feeds](https://loic-sharma.github.io/BaGet/private-feeds/)
* And more!

Stay tuned, more features are planned!

## Develop

1. Install [.NET Core SDK](https://www.microsoft.com/net/download) and [Node.js](https://nodejs.org/)
2. Run `git clone https://github.com/loic-sharma/BaGet.git`
3. Navigate to `.\BaGet\src\BaGet.UI`
4. Install the frontend's dependencies with `npm install`
5. Navigate to `..\BaGet`
6. Start the service with `dotnet run`
7. Open the URL `http://localhost:5000/v3/index.json` in your browser

## Helm

### Package/Install

* Clone repo then run helm package

```
helm package baget
```

You can then push that chart to any helm repository you want and install from there

```
helm install -g myrepo/baget
```

* Install without package

```
helm install -g baget
```

Modify the isntallation by either editing the `values.yaml` file or passing the parameters you want to change with `--set`

```
helm install -g baget --set fullname=nuget,persistence.enabled=true,persistence.stoageClass=someclass
```

### Configure

| Parmeter                      | Description                                             | Default                           |
|-------------------------------|---------------------------------------------------------|-----------------------------------|
| `fullname`                    | Name of the deployment                                  | `baget`                           |
| `gracePeriod`                 | terminationGracePeriodSeconds setting                   | `10`                              |
| `image`                       | Name of the image to deploy                             | `loicsharma/baget`                |
| `imageVersion`                | Version of the image to deploy                          | `latest`                          |
| `namespaceOverride`           | Override context namespace                              | ``                                |
| `replicas`                    | Number of pods to deploy                                | `1`                               |
| `env.apiKey`                  | API key users will use to auth                          | ``                                |
| `env.storageType`             | Type of storage to be used                              | `FileSystem`                      |
| `env.storagePath`             | Path to use for storage                                 | `/var/baget/packages`             |
| `env.databaseType`            | Type of database                                        | `Sqlite`                          |
| `env.databaseConnectionString`| Connection string for db                                | `Data Source=/var/baget/baget.db` |
| `env.searchType`              | Type of search to carry out                             | `Database`                        |
| `ingress.enabled`             | Enable and create an ingress                            | `false`                           |
| `ingress.hosts`               | External DNS of the app                                 | `name: "", tls: false, secret: ""`|
| `persistence.acceesMode`      | Storage access mode                                     | `ReadWriteOnce`                   |
| `persistence.enabled`         | Enable and use persistent storage                       | `false`                           |
| `persistence.path`            | Path to mount pvc                                       | `/var/baget`                      |
| `persistence.size`            | Size of the pvc                                         | `10G`                             |
| `persistence.storageClass`    | Storage class for pvc                                   | ``                                |
| `persistence.volumeName`      | Name of existing pv                                     | ``                                |
| `resources.requests`          | Compute resource requests                               | `mem: 100Mi, cpu: 100m`           |
| `resources.limits`            | Compute resource limits                                 | `mem: 250Mi, cpu: 200m`           |
| `service.enabled`             | Enable and create service                               | `true`                            |
| `service.NodePort`            | Specify Node port (relies on `service.type: NodePort`)  | ``                                |
| `service.serviceName`         | Name of the service                                     | `{{ .Values.fullname }}-svc`      |
| `service.type`                | Type of service to create                               | `ClusterIP`                       |
