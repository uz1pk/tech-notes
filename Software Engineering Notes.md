- [Sidecar](https://learn.microsoft.com/en-us/azure/architecture/patterns/sidecar)
	- Problem context, an application has tasks a little unrelated to the core application like monitoring, integratoins, platform abstractions, etc. Introducing a completely different service for this add complexity and latency, while running in the same process/node creates interdependence on the main application (language, configs, etc) and creates a single point of failure, if one fails/dies they all do
	- Solution: 
- [Daemon](https://superuser.com/questions/1700596/what-is-a-daemon-in-software)
	- A background process (independent of any others) that does tasks or accepts tasks (network requests / socket requests) to do some job
		- think docker CLI talking to the docker daemon (daemon manages kernel level things)
- [RPC](https://www.geeksforgeeks.org/remote-procedure-call-rpc-in-operating-system/) (Remote Procedure Call)
	- A protocol / abstraction for calling a function / subroutine on another machine. Abstracts away how the request is made at the network level (TCP, UDP, and other transport layer things)
	-[ JSON-RPC](https://www.jsonrpc.org/) A specification built on top of RPC to use JSON as the communication format when making RPC calls
### Containers (Docker)
- Container is a software unit that includes the configuration, bundle, runtime info, code, tools, libs, etc. into a singular exe (it's a process)
- Docker images are the configuration needed for a container, while a container is an instance of the image
- Containers are processes with their own namespaces, resources, etc. They run on a machine that has the docker-engine for executing the container

### Kubernetes (k8s)
