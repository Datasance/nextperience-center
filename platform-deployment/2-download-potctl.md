# Download And Install potctl

## Install potctl on Mac

Mac users can use Homebrew:

```bash
brew tap datasance/potctl
brew install potctl
```

## Install potctl on Windows

The Windows binary can be downloaded from [Datasance Packages](https://github.com/Datasance/potctl/releases/download/v1.0.0/potctl.exe).

### Prepare Windows

In order to use `potctl` to deploy an ECN locally on Windows we will need to configure Docker to run Linux containers:

- Install [docker desktop for windows](https://download.docker.com/win/stable/Docker%20Desktop%20Installer.exe)
- Enable Hyper-V in Powershell `Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart`
- Ensure that docker is running with [Linux containers mode](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)

## Install potctl on Linux

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

## Verify potctl Installation

Run `potctl version` to verify you have successfully installed the CLI.

<aside class="notifications note">
  <h3><img src="/images/icos/ico-note.svg" alt="">Need more details aboout potctl</h3>
  <p>Check out [Getting familiar with potctl ](https://docs.datasance.com/#/./ioFog_3.0/potctl/getting-familiar)  and [potctl reference ](https://docs.datasance.com/#/./ioFog_3.0/reference-potctl/reference-kinds). </p>
 
</aside>