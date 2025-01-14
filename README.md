[![Release](https://img.shields.io/github/release/amerello/kube-tasks.svg)](https://github.com/amerello/kube-tasks/releases)
[![Travis branch](https://app.travis-ci.com/amerello/kube-tasks.svg?branch=master&status=passed)](https://app.travis-ci.com/github/amerello/kube-tasks)
[![Docker Pulls](https://img.shields.io/docker/pulls/amerello/kube-tasks.svg)](https://hub.docker.com/r/amerello/kube-tasks/)
[![Go Report Card](https://goreportcard.com/badge/github.com/amerello/kube-tasks)](https://goreportcard.com/report/github.com/amerello/kube-tasks)
[![license](https://img.shields.io/github/license/amerello/kube-tasks.svg)](https://github.com/amerello/kube-tasks/blob/master/LICENSE)

# Kube tasks

A tool to perform simple Kubernetes related actions

## Commands

### Simple Backup

Usage:
```
Backup files to cloud storage

Usage:
  kube-tasks simple-backup [flags]

Flags:
  -b, --buffer-size float   in-memory buffer size (in MB) to use for files copy (buffer per file) (default 6.75)
  -c, --container string    container name to act on
      --dst string          destination to backup to. Example: s3://bucket/backup
  -n, --namespace string    namespace to find pods
  -p, --parallel int        number of files to copy in parallel. set this flag to 0 for full parallelism (default 1)
      --path string         path to act on
  -l, --selector string     selector to filter on
      --tag string          tag to backup to. Default is Now (yyMMddHHmmss)
  -s, --skip-error-files    Skip error files (default true)
```

Example: Backup Jenkins
```
kube-tasks simple-backup -n default -l release=jenkins -c jenkins --path /var/jenkins_home --dst s3://amerello-jenkins-data
```

### Wait for Pods

Usage:
```
Wait for a given number of ready pods

Usage:
  kube-tasks wait-for-pods [flags]

Flags:
  -n, --namespace string   namespace to find pods
  -r, --replicas int       number of ready replicas to wait for (default 1)
  -l, --selector string    selector to filter on
```

Example: Cassandra Repairer init container
```
kube-tasks wait-for-pods -n prod -l release=prod-cassandra --replicas=3
```

### Execute

Usage:
```
Execute a command in a container. Only executes the command in the first pod

Usage:
  kube-tasks execute [flags]

Flags:
      --command string     command to execute in container
  -c, --container string   container name to act on
  -n, --namespace string   namespace to find pods
  -l, --selector string    selector to filter on
```

Example: Cassandra Repairer
```
kube-tasks execute -n prod -l release=prod-cassandra --command "nodetool repair --full"
```
