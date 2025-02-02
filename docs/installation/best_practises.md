# Installation best practises

It is highly recommended to set up an encrypted connection when accessing the Tracardi API. One way to do this is to
encode the traffic as it is sent to the load balancer, but leave the internal cluster traffic decoded. This allows for
secure communication with the API while still allowing for efficient communication within the cluster. If you prefer to
have encoded traffic throughout the entire cluster, you will need to prepare HTTPS versions of the Tracardi Docker
images. This will ensure that all traffic, both external and internal, is encrypted for added security. It is important
to carefully consider the security measures in place when working with sensitive data, and setting up an encrypted
connection to the Tracardi API is an important step in protecting this information. See
the [documentation on how to do it](../configuration/tracardi_ssl.md).

## Separation of track server and GUI API

Best practice is to have a setup of 2 Tracardi API clusters. One for GUI and one for event collection.

The event collecting cluster, the one available on the Internet, should be configured with environment
variable `EXPOSE_GUI_API` set to `no`. This way the publicly available API will have only `/track` endpoint available.
All other endpoints that are required by the GUI will be disabled. This cluster will be used for event collection from
the website or other sources available on the Internet.

Another cluster, the one available in the internal network, or available on the Internet but restricted to certain set
of IPs, should have the environment variable `EXPOSE_GUI_API` set to `yes`. This cluster will be used by GUI to control
Tracardi.

## Scaling

Scaling refers to the process of increasing or decreasing the capacity of a system to meet the demands of incoming
traffic. In the context of Tracardi, scaling may involve increasing the number of servers or instances (tracardi-api) in
order to handle a larger volume of event tracking requests.

Tracardi is a stateless system, meaning that it does not rely on storing data locally in order to function. This makes
it easier to scale, as new instances of Tracardi can be added or removed without affecting the overall functioning of
the system. We recommend using Kubernetes, a container orchestration platform, to manage the scaling and maintenance of
the Tracardi cluster. Tracardi does not require long-lasting sticky sessions, so it can be easily killed and re-launched
without disrupting the cluster.