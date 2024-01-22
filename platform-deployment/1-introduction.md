
# Introduction

Edge Compute Networks ('ECNs') are complicated things. At a high level, they are composed of a Control Plane and a set of Agents.

Setting up and managing an ECN out of an arbitrary set of remote hosts can seem like a daunting task. That's why we are going to use `potctl`. `potctl` is a CLI that simplifies the orchestration of ECNs as well as the deployment of microservices on those ECNs. In this guide, we will step through the process of creating a real, distributed ECN across a set of remote hosts.

In this guide, we will go through deployment onto a Kubernetes cluster.  we also need to setup the network, but we only need to prepare the Agent hosts for SSH access.


