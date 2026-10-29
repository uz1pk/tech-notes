
- Registry is a hub of all images. Repositories are subsets within a registry that represent a set of images from the overall registry. Likely for permissioning, scoping, etc.
- Docker uses [linux namespaces](https://medium.com/@teddyking/linux-namespaces-850489d3ccf) under the hood for isolation
- Docker layers are immutable snapshots of filesystem changes that occur when building / deploying an image
- 