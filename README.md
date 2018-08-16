# Fluent-bit Daemonset for Kubernetes Logging

[![Build Status](https://jenkins.migrations.cnct.io/buildStatus/icon?job=pipeline-fluent-bit/master)](https://jenkins.migrations.cnct.io/job/pipeline-fluent-bit/job/master)

[Fluent-bit](http://fluentbit.io/) daemonset dependencies for Kubernetes
logging. The helm chart and docker image for this repository are located at:
https://quay.io/application/samsung_cnct/fluent-bit and
https://quay.io/repository/samsung_cnct/fluent-bit-container respectively.

Currently this daemonset reads [Docker logs](https://docs.docker.com/engine/admin/logging/overview/) from `/var/log/containers` and [journald logs](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html) from `/var/log/journal`. It adds Kubernetes metadata to the logs and writes them to stdout.
It is included in our [logging](https://github.com/samsung-cnct/chart-logging) pipeline.

## How to install on running Kubernetes cluster with `helm`
Install Helm and the Helm registry plugin with [these](https://github.com/app-registry/appr-helm-plugin/blob/master/README.md#install-the-helm-registry-plugin) instructions.

```
helm registry install quay.io/samsung_cnct/fluent-bit
```

## Plugins

#### Systemd Input Plugin

This input plugin reads from /var/log/journal, which contains kernel, dockerd, and rkt logs, among others. It is new as of v0.12.
More informaton on this plugin can be found at:
http://fluentbit.io/documentation/0.12/input/systemd.html

#### Tail Input Plugin

This input plugin monitors text files as matched by a specified Path; in this case, `/var/log/containers/*.log`, excluding `/var/log/containers/fluent*.log`. More information on this plugin can be found at: http://fluentbit.io/documentation/0.12/input/tail.html

#### Kubernetes Metadata Filter

This filter adds the following data into the body of the log.
* namespace
* pod id
* pod name
* labels
* host
* container name
* docker container id

For more information on the filter or to see a list of configuration options: http://fluentbit.io/documentation/0.12/filter/kubernetes.html 
