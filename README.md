# Getting Started Guide for Installation Datasane PoT on TD-SYNNEX Nextperience Demo Center


Edge Compute Networks ('ECNs') are complicated things. At a high level, they are composed of a Control Plane and a set of Agents.

Setting up and managing an ECN out of an arbitrary set of remote hosts can seem like a daunting task. That's why we are going to use `potctl`. `potctl` is a CLI that simplifies the orchestration of ECNs as well as the deployment of microservices on those ECNs. In this guide, we will step through the process of creating a real, distributed ECN across a set of remote hosts.

In this guide, we will go through deployment onto a Kubernetes cluster.  we also need to setup the network, but we only need to prepare the Agent hosts for SSH access.

<figure>
  <img src="/images/nextperience.png" alt="">
  <figcaption>Demo Setup.</figcaption>
</figure>

##  1. Downdload and Install potctl

- ## Install potctl on Mac

Mac users can use Homebrew:

```bash
brew tap datasance/potctl
brew install potctl
```

- ## Install potctl on Windows

The Windows binary can be downloaded from [Datasance Packages](https://github.com/Datasance/potctl/releases/download/v1.0.0/potctl.exe).

- ### Prepare Windows

In order to use `potctl` to deploy an ECN locally on Windows we will need to configure Docker to run Linux containers:

- Install [docker desktop for windows](https://download.docker.com/win/stable/Docker%20Desktop%20Installer.exe)
- Enable Hyper-V in Powershell `Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart`
- Ensure that docker is running with [Linux containers mode](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)

- ## Install potctl on Linux

The Debian package can be installed like so:

```bash
wget -qO- https://downloads.datasance.com/datasance.gpg | sudo tee /etc/apt/trusted.gpg.d/datasance.gpg >/dev/null
echo "deb [arch=all signed-by=/etc/apt/trusted.gpg.d/datasance.gpg] https://downloads.datasance.com/deb stable main" | sudo tee /etc/apt/sources.list.d/datansance.list >/dev/null
sudo apt update
sudo apt install potctl -y

```

And similarly, the RPM package can be installed like so:

```bash
cd /etc/yum.repos.d ; curl https://downloads.datasance.com/datasance.repo -LO
sudo yum update
sudo yum install potctl
```

- ## Verify potctl Installation

Run `potctl version` to verify you have successfully installed the CLI.

<aside class="notifications note">
  <h3><img src="/images/icos/ico-note.svg" alt="">Need more details aboout potctl</h3>
  <p> </p>
 </aside>

Check out [Getting familiar with potctl](https://docs.datasance.com/#/./ioFog_3.0/potctl/getting-familiar) and [potctl reference ](https://docs.datasance.com/#/./ioFog_3.0/reference-potctl/reference-kinds).




##  2. Prepare Network

Once we have a set of Kubernetes cluster we need to make sure that we have appropriate firewall rules for the ioFog components that we will start deploying on those remote hosts.

The following is an exhaustive list of the ingress firewall rules required for any ioFog Edge Compute Network.

| Component  | Protocol/Port | Description                                                       |
| ---------- | ------------- | ----------------------------------------------------------------- |
| Controller | tcp:51121     | Controller API to be accessible from Agents and potctl.         |
| Controller | http:80       | ECN Viewer to be accessible by ECN admins or users.               |
| potctl   | tcp:22        | SSH access to install Agent on remote hosts. |

Please keep in mind that once we start [exposing microservices using public ports](https://docs.datasance.com/#/./ioFog_3.0/applications/microservice-exposing) in our ECNs, there will be additional ports that we expect to be accessible from outside of the network.





##  3. Prepare your Kubernetes Cluster for PoT Control Plane Deployment

Some components of an Edge Compute Network ('ECN') can be deployed on Kubernetes. These component include Controllers. `potctl` will also install ioFog Operator to assist in the Kubernetes deployed Control Plane. Resources for routing will also be deployed.

The ioFog platform can be installed easily on a managed Kubernetes Cluster provided by e.g. AWS, GCP, or Azure. Minikube is supported as well.

Using lightweight Kubernetes implementations such as MicroK8s or K3s are not fully supported. They will likely fail because those platforms do not assign host or IP addresses to Load Balancer services.

- ## Kube Config

If we are familiar with `kubectl`, we will know that it relies on a configuration file typically found in `~/.kube/config` which contains credentials for it to connect to the Kubernetes API Server.

`potctl` similarly relies on this configuration file. We need to make sure it is present on the host that we intend to use `potctl` from. The file can be saved anywhere on the host but it is recommended to keep it in `~/.kube/config` because all of this guide's examples assume it to be there.

- ## RBAC

In order for `potctl` or Helm to do its thing with our Kubernetes cluster, we will have to make sure we have the right [RBAC permissions](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).

The way we add these permissions will depend on our respective Kubernetes provider. Ultimately, we need the User Account associated with our `~/.kube/config` configuration file to have at least the following permissions:

```yaml
Resources                               Verbs
---------                               -----
configmaps                              [*]
persistentvolumeclaims                  [*]
pods                                    [*]
secrets                                 [*]
serviceaccounts                         [*]
services                                [*]
deployments.apps                        [*]
rolebindings.rbac.authorization.k8s.io  [*]
roles.rbac.authorization.k8s.io         [*]
apps.iofog.org                          [list get watch]
controlplanes.iofog.org                 [list get watch]
```

If in doubt, we can use the default cluster role `cluster-admin`.



##  4. Prepare Remote Agent Hosts

ECN's are deployed using `potctl`. The following steps will ensure that our remote Agent hosts are ready to have `potctl` deploy ECN components onto them remotely.

- ## Add SSH RSA Public Key to the Remote Host

`potctl` will SSH into our remote hosts using an RSA SSH key-pair we specify.

We add the public key of the key-pair that we intend to use with `potctl` to the remote host to deploy ECN components onto. For example, we could run a command like this:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub <username>@<controller-or-agent-hostname>
```

We do this for Controller host (only when we intend to deploy Controller directly, i.e. not to Kubernetes cluster) and all the agents. Node this is the key we will later use to deploy Controller and Agent using `potctl`, please keep track of it.

- ## Add the Remote Host's User to Sudo Group

`potctl` will need to run certain commands as sudo. In order to do this without using the root user, we must add the user we intend to use on the remote host to the sudo group. Run this command on all the remote host:

```bash
usermod -aG sudo $USER
```

- ## Allow Sudoers to Sudo without Password

In order to prevent `potctl` from failing its SSH sessions, we need to ensure that the sudoers in the remote host can run commands without having to enter a password. Note that `potctl` is unable to prompt the user for passwords.

We can achieve this by editing the sudoers file by first running `su visudo` on the remote host. This will open up the sudoers file for editing. Make sure this line is present in the file `%sudo ALL=(ALL) NOPASSWD:ALL`.



- [6- Deploy Control Plane Using potctl]
- [7- Deploy Agent]


