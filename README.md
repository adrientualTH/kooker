# Kooker

**K**ooker with a **K** is derived from the word *Cooker*. Like a cooker is used for 
making delicious recipes, Kooker cooks a (hopefully tasty) Punch recipe on top of K3D.

In short: Kooker deploys a Kubernetes cluster together with services such as Kafka, 
Elastic, Minio, Clickhouse.. using a tool called *kastctl* so that you can start 
kubernetes applications in minutes on any laptop as long as you have docker installed.

Kooker is developped by the Punch Team with best effort support and is not included officially 
in Punch product. We encourage you to contribute.  

## Kooker Essential Design

Kooker first bootstraps a k3d cluster, and next installs a number of additional components, the
ones you need. You can install any Kubernetes application as long as it is provided through helm
charts and container images. 

To play all these helm charts, and provide you with a simpler global configuration file (rather than
simply execute a bunch of disparate helm charts), Kooker uses a tool called *kastctl*.

Kastctl is developed by the (Kast) Thales teams and is a lightweight helm executor. The benefit
is to let you define your platform blueprint using a single simpler yaml file called *kpack.yaml*. 
Kastctl is publicly available as a (macos|linux|windows) application and will be automatically
downloaded at startup time.

## Getting Started

First type in 
```sh
./install.sh 
```
That generates a small setup environment script. Activate it as follows:
```sh
source activate.sh 
```

The default procedure is then the following:

```sh
# download deploy and start the required components. Skip the
# --interactive option to install the default profile as defined
# in kpack/kpack.yaml
kooker --interactive start 

# Expose all the reruied service to your host. This requires a sudoer
# password to patch your /etc/host file.
kooker expose 

# Just checking
kooker status

# Get the information about what is there and what are the exposed URLs
kooker info
```

Once components are up, you can visit the various UI, for instance http://dashboard.punch:8080
(if you selected the punch board application). 

Note that using the default profile, all login password are set to punch (user) and punchplatform (password).


## Custom Deployment

You have three ways to work with kooker: 

### Install a default platform

Simply type in 
```sh
kooker start
```
That command installs all the components as specified in the kastctl [kpack/pack.yaml](./kpack/kpack.yaml)
configuration file if the *kooker kpack* command has never been used. Have a look at it, it is easy to understand.

### Interactively Install What you Want

Instead you can type in: 
```sh
kooker --interactive start
```
The same kpack.yaml file is used but you will be prompted to install only the components that 
you need from it. 

### Define You Own Profile 

You can define your own kpack.yaml file to include only the components you want. This requires
a kastcl documentation guide that is planned soon. 

In order to use your kpack file run the following command:

```sh
kooker kpack <kpack_file>
```
### Registries configuration file

You can add a *registries.yaml* file in the kooker directory containing images registries configuration.  Note that this file
is only used during the cluster creation.


## Typical Kooker Users and Usages 

Here are the Kooker users :

- Punch dev team : to develop and test Punch application, and to deploy third party components locally (elastic, Kafka ..)
- Punch and Kast professonal services: to reproduce and investigate production issues.
- Any users which need a lightweight kube, and to deploy his own apps on it.

Note that Thales users benefit from access to Kast private image and helm charts repositories. 
This makes it easy to use Kooker to deploy components using the ones provided by Kast, and also configured
the way Kast propose to do it. Do not forget to configure Kast repo before using it (cf kastctl documentation)

If you have access to the Kast helm charts registry, please add the Kast helm repository manually as a prerequisite and run the command:

```sh
kooker kpack kpack/kpack_private.yaml
```

Non Thales user may only refer to their own or to public images and helm charts. 

As for Punch images and charts, they are publicly available from github repositories, hence usable by
everyone. 

## Requirements

### Hardware
- 4 vCPU
- 4.5 Go RAM
- 20 Go Disk (15Go are used by docker images)

You should increase those minimal requirements based on your usage of Kooker.

### Software
- docker
- curl
- bash

## Contributing 

Update the kpack file conf/kpack.yaml to add a new component using a kast or a 
custo helm chart

