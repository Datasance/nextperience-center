<aside class="notifications tip">
  <h3><img src="/images/icos/ico-tip.svg" alt="">Not interested in using Kubernetes?</h3>
  <p>There are two flavours of Control Plane deployments - Remote and Kubernetes. This guide will focus on deploying a Remote Control Plane on a Kubernetes cluster. Go to <a href="#/./ioFog_3.0/platform-deployment/remote-control-plane">Remote - Deploy Control Plane</a> to deploy the Control Plane on a Linux host instead. Keep in mind that in such case, it will be necessary to prepare the host for Controller as well.</p>
  <p>Also, this guide will use potctl to deploy the Control Plane on the cluster. To use Helm instead, go to <a href="#/./ioFog_3.0/platform-deployment/kubernetes-helm"> Kubernetes - Deploy Control Plane Using Helm</a>.</p>
</aside>

# Kubernetes - Deploy Control Plane Using potctl

Every Edge Compute Network ('ECN') starts with a Control Plane that allows us to manage our ECN's resources.

In this guide, our Control Plane will deploy a single Controller instance.

<aside class="notifications note">
  <h3><img src="/images/icos/ico-note.svg" alt="">We use YAML to define ioFog resources</h3>
  <p>The following procedures will define resources in YAML for potctl to consume. Specification of those YAML resources can be found <a href=#/./ioFog_3.0/reference-potctl/reference-control-plane>here</a>.</p>
</aside>

## Deploy a Control Plane on Kubernetes

Create a template of controlplane.yaml like so:

```bash
echo "---
apiVersion: datasance.com/v1
kind: KubernetesControlPlane
metadata:
  name: albatros-1
spec:
  iofogUser:
    name: Foo
    surname: Bar
    email: user@domain.com
    password: iht234g9afhe
  config: ~/.kube/config" > /tmp/controlplane.yaml
```

Make sure to specify the correct value for the `config` field. Here we implicitly use the default namespace. Note that potctl will deploy to the Kubernetes namespace that it is configured to use through the `-n` flag or to the default namespace we set via `potctl configure current-namespace ...`. This means that by following these examples, we end up installing the Control Plane in `default` namespace on the cluster. Therefore it is recommended to use a namespace instead.

Once we have edited the fields to our liking, we can go ahead and run:

```bash
potctl deploy -f /tmp/controlplane.yaml
```

Naturally, we can also use `kubectl` to see what is happening on the Kubernetes cluster.

```bash
kubectl get all
```

The next section covers how to do the same thing we just did, but on a remote host instead of a Kubernetes cluster. We can <a href=#verify-the-deployment>skip ahead</a>.

## Verify the Deployment

We can use the following commands to verify the Control Plane is up and running:

```bash
potctl get controllers
```

```bash
potctl describe controller alpaca-1
```

```bash
potctl describe controlplane
```

<aside class="notifications tip">
  <h3><img src="/images/icos/ico-tip.svg" alt="">Where to go from here?</h3>
  <p>Having our Control Plane up and running, we can now go to <a href="#/./ioFog_3.0/platform-deployment/setup-your-agents">Setup Agents</a> guide to deploy our Agents and finalize the ECN deployment.</p>
</aside>

<aside class="notifications contribute">
  <h3><img src="/images/icos/ico-github.svg" alt="">See anything wrong with the document? Help us improve it!</h3>
  <a href="https://github.com/eclipse-iofog/iofog.org/edit/develop/content/docs/3.0/platform-deployment/kubernetes-potctl.md"
    target="_blank">
    <p>Edit this page on Github!</p>
  </a>
</aside>
