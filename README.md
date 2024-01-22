# Getting Started Guide for Installation Datasane PoT on TD-SYNNEX Nextperience Demo Center


Edge Compute Networks ('ECNs') are complicated things. At a high level, they are composed of a Control Plane and a set of Agents.

Setting up and managing an ECN out of an arbitrary set of remote hosts can seem like a daunting task. That's why we are going to use `potctl`. `potctl` is a CLI that simplifies the orchestration of ECNs as well as the deployment of microservices on those ECNs. In this guide, we will step through the process of creating a real, distributed ECN across a set of remote hosts.

In this guide, we will go through deployment onto a Kubernetes cluster.  we also need to setup the network, but we only need to prepare the Agent hosts for SSH access.

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

- [3- Prepare Network]
- [4- Prepare your Kubernetes Cluster for PoT Control Plane Deployment]
- [5- Prepare Remote Agent Hosts]
- [6- Deploy Control Plane Using potctl]
- [7- Deploy Agent]


