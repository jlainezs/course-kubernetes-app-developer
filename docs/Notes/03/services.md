# Services

# NodePort
Forwards requests to a container:

user -> node -> pod -> container

This allows to access, pe., a web service from outside.

Defining a NodePort service involves three ports, named from the service pov:

- Target port: the port where the service is running. If not specified Port value is used.
- Port: the port in the service itself. MANDATORY.
- NodePort: the port in the node used to access the service externally. Ranges from 30000-32767. If not specified a free random port in the range is assigned.

The pod is identified through selectors. As there could be several ports selected, the service acts as an internal load balancer between those ports.

When the service runs in a multiple nodes cluester, it is expanded to all nodes in the cluster, so the service is available on any node cluster ip. 

# ClusterIP
Creates a virtual IP inside the cluster to enable communication between different services.

# LoadBalancer
Provisions a load balancer to access application in supported cloud providers.
