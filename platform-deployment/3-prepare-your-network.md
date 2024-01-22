# Prepare Network

Once we have a set of Kubernetes cluster we need to make sure that we have appropriate firewall rules for the ioFog components that we will start deploying on those remote hosts.

The following is an exhaustive list of the ingress firewall rules required for any ioFog Edge Compute Network.

| Component  | Protocol/Port | Description                                                       |
| ---------- | ------------- | ----------------------------------------------------------------- |
| Controller | tcp:51121     | Controller API to be accessible from Agents and potctl.         |
| Controller | http:80       | ECN Viewer to be accessible by ECN admins or users.               |
| potctl   | tcp:22        | SSH access to install Agent on remote hosts. |

Please keep in mind that once we start [exposing microservices using public ports](https://docs.datasance.com/#/./ioFog_3.0/applications/microservice-exposing) in our ECNs, there will be additional ports that we expect to be accessible from outside of the network.

