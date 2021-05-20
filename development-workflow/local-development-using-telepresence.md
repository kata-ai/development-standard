## Introduction

Telepresence is an open source tool that lets you run a single service locally, while connecting that service to a remote Kubernetes cluster, so we can treat our local development machine as if it's part of Kubernetes cluster.

## Context and Goals

Currently to develop services we can run the entire platform locally via Docker Compose. This gives us a fast dev/debug cycle. However, it's less realistic since we are not running our services actually inside Kubernetes, and there are cloud services we might use (e.g., a database) that might not be easy to use locally. With Telepresence we can run everything in a remote Kubernetes cluster and do live coding/debugging.


## Debugging

### Prerequisite
1. [Install Google Cloud SDK](https://cloud.google.com/sdk/docs/install)
2. [Install kubectl CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
3. [Install Telepresence](https://www.telepresence.io/reference/install)

### Configuration
Access Kata.ai staging Kubernetes cluster with `gcloud` command.
Ask Infra team to get Kata.ai staging gke-engineer.json service account to get access to `kubernetes` resource.

Then run this following commands to setup connection to `kubernetes` staging.

```shell-session
gcloud auth activate-service-account --key-file=gke-engineer.json
gcloud config set project kata-staging-238005
gcloud config set container/cluster kata-staging-kubernetes-cluster
gcloud config set compute/zone asia-southeast1-a
```

	
Next we need to setup kubectl command to access kata-ai-staging cluster.

```shell-session
gcloud container clusters get-credentials kata-staging-kubernetes-cluster --zone asia-southeast1-a
kubectl config use-context gke_kata-staging-238005_asia-southeast1-a_kata-staging-kubernetes-cluster
```

Verify the configuration above, by running.

```bash
kubectl get pod -n platform-2018
```


### Exposing local service with Telepresence

If you want to swap the `postbote` service in kubernetes with local application running in port 5000. Run this following command.

```
telepresence --swap-deployment postbote --namespace platform-2018 --expose 5000
```

Now test `postbote` service that served from local port.
```bash
curl https://posbote.katalabs.io/info
```

Let's try kill the telepresence process by hitting `Ctrl+C` and wait for few second to swap back to old code.

## Bugs and Limitations
Here's a list of bugs and limitations we found during our exploration and trying out telepresence workflow. The lists might be changed overtime with the update of the telepresence version.

1. Error in command line after telepresence launched.
```
Looks like there's a bug in our code. Sorry about that! 
 
Traceback (most recent call last): 
 File "/usr/local/bin/telepresence/telepresence/cli.py", line 135, in crash\_reporting 
 yield 
 File "/usr/local/bin/telepresence/telepresence/main.py", line 71, in main 
 env, pod\_info = get\_remote\_env(runner, ssh, remote\_info)
 File "/usr/local/bin/telepresence/telepresence/remote\_env.py", line 32, in get\_remote\_env
 json\_data = runner.get\_output(
 File "/usr/local/bin/telepresence/telepresence/runner/runner.py", line 479, in get\_output
 output = self.\_run\_command\_sync(
 File "/usr/local/bin/telepresence/telepresence/runner/runner.py", line 437, in \_run\_command\_sync
 raise CalledProcessError(
subprocess.CalledProcessError: Command '\['kubectl', '--context', 'default', '--namespace', 'platform-2018', 'exec', 'wache-256aecfb76984b2486619760dbd5e9b9', '--container', 'wache', '--'
, 'python3', 'podinfo.py'\]' returned non-zero exit status 1.
Would you like to file an issue in our issue tracker? You'll be able to review and edit before anything is posted to the public. We'd really appreciate the help improving our product. \[Y
/n\]:
```
**Workaround**: If you encounter this issue don't issue any key (Y/n) because it will exit the process.

2. Old pod doesn't spawn back when telepresence exited.
**Workaround**: Rerun the pipelines in gitlab CI.

## References
- [Telepresence - Introduction](https://www.telepresence.io/discussion/overview)
