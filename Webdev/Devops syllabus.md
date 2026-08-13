## Linux / Bash scripting

|   |   |   |
|---|---|---|
|Easy|Medium|Hard|
|Basic bash commands|||
|aliases, .bashrc/.zshrc ,|||
|interactive vs non interactive shells|Process Management||
||Cron jobs||
||File transfer - ftp, scp||
||File permissions||
|Creating your own bash script - variables, loops, conditionals, functions|||
||args in bash scripts||
||stderr, stdout, File descriptors, piping||
||grep||
||Creating a CLI in Node.js||

Projects

1. Create a script that `ssh's` into a VM and pull the latest code and restarts it
2. Do the same using ssh2

## VMs/EC2/GCP VMs

|   |   |   |
|---|---|---|
|easy|medium|hard|
|What are VMs?|||
|Creating a VM in various cloud providers. Specs, pricing …|||
|GUI vs clis for creating VMs|Keypairs and ssh’ing into VMs||
||Deploying a `non containerized` app to a VM||
|Process management|||
|Reverse proxies|||
||Certificate management||
||Load balancers||

## ASGs/MIG

|   |   |   |
|---|---|---|
|Easy|Medium|Hard|
|What is autoscaling, why do you need it?|||
||Images, Target groups, ASGs, LBs, Autoscaling policies||
||MIG Internals||

## Containerization

|   |   |   |
|---|---|---|
|What is a container|||
||What is docker||
|||Docker vs other container runtimes|
||How to dockerize your app, WORKDIR, CMD, RUN, ENV, COPY, ARG||
|||Volumes and networks|
|Publishing to dockerhub, other registries|||
||Running a docker container in a VM, exposing it over the internet||
|||Multi stage builds|
||docker-compose||

## ECS (Warming up for k8s)

|   |   |   |
|---|---|---|
|easy|medium|hard|
|What is ECS, common use cases|||
||ECR vs Dockerhub||
||Setting up Task definitions||
||Serverless vs EC2 instances for deploying , pros and cons of either||
||Autoscaling policies||

## Kubernetes

|   |   |   |
|---|---|---|
|Easy|Medium|Hard|
|Introduction to Kubernetes, clusters, nodes, pods, worker node vs control plane|||
|||Kubernetes architecture|
||Local deployment using kind/minikube||
||Replicasets, Deployments||
||Networking Concepts - Services, Types of services||
|||Ingress, Ingress controllers|
||Certificate management||
||Storage, PVs, PVCs, StorageClasses, Static vs dynamic provisioning||
|||StatefulSets, DaemonSets, Jobs and CronJobs|
||Conifgmaps and secrets||
||Kubernetes Policies, Role-Based Access Control, CRDs||
|||HPAs, VPA, Cluster autoscaler|
||EKS, eksctl, eks best practises||
||Extra - k9s, kubctx||

## Iac, Terraform

|   |   |   |
|---|---|---|
|Easy|Medium|Hard|
|What is Iac|||
||Iac using bash scripts, aws cli/eksctl||
||Various iac tools, ansiable vs terraform.||
||Installing tf, What is a resource, HCL syntax, Providers||
||Running Terraform Commands (init, plan, apply, destroy)||
|||Managing state, variables, counts|
|||Terraform with AWS|
|||Remote state on S3|
||||

## Monitoring

|   |   |   |
|---|---|---|
|Easy|Medium|Hard|
|Introduction to monitoring, why do you need to monitor your systems|||
||Datadog vs newrelic vs sentry.  <br>Logging vs process observability vs Infrastructure observability. p95, p99.||
||Self hosted monitoring using prometheus. metrics, counters, gauges, histrograms, promQL, Graphs in Prom||
||Pulling vs pushing metrics, Push gateway||
||Grafanna for charting, observability.||
||Prometheus and grafanna service discovery in a kubernetes cluster||

### Package managers (helm)

|   |   |   |
|---|---|---|
|easy|medium|hard|
||Why package managers, what is helm?||
||Installing helm cli, values and templates||
||Helm charts, Dependency management||
||Helm hubs, repos, custom values,||
|||Release management in Helm 2.0 and 3.0|
|||Templates, Go templating logic,|
|||functions, pipelines, with blocks, creating and uploading charts|
||Exploring other package mangers (glasskube)||

### CI/CD in github

|   |   |   |
|---|---|---|
|Easy|medium|Hard|
|What is CI/CD?|||
|Github CI CD yaml format, Github actions|||
|Using CI/CD to deploy to a VM|||
||Using CI/CD to provision infrastructure using Iac||
||Using CI/CD to deploy to a kubernetes cluster||

## Gitops

|   |   |   |
|---|---|---|
|Easy|Medium|Hard|
|What is GitOps?  <br>Why gitops|||
||Creating aws resources using gitops||
||Creating/Updating k8s clusters||
||GitOps using ArgoCD||
|||Gitops in a multi cluster environment using ArgoCD|

## CDNs and Object stores

|   |   |   |
|---|---|---|
|Easy|Medium|Hard|
|What are object stores, Why object stores?|||
|Intro to CDNs|||
||S3, R2 for assets||
||Cloudfront as a CDN||
||Attaching a custom domain to your CDN, certificate management||

Project

1. Building a serverless provider (aws lambda) / repl.it
	1. http layer, cli layer, auth
2. Building a distributed video transcoder / image resizer
3. Building a alerts manager (better stack) and status page provider