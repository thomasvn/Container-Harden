## COMPLIANCE
- STIG base image
- STIG application
- NIST800-53v4 moderate controls plus FedRAMP+ IL6controls
- NISTSP800-190 and Center for Internet Security (CIS) Docker Benchmark

## IMAGE CREATION
(all below instructions apply to the `Dockerfile`)
- The Container Image Must Be Built with the SSH Server Daemon Disabled
- The Container Image Must Be Created to Execute as a Non-Privileged User 
    - no root user (UID 0)
    - In Dockerfile `RUN groupadd -r user && useradd -r -g user user` and `USER user`
- The Container Image Must Have Permissions Removed from Executables that Allow a User to Execute Software at Higher Privileges
    - no executables with `setuid` or `setgid` bit set
- The Container Image Must Be Built Using Commands that Result in Known Outcomes
    - do not use `ADD` and `RUN` on shell scripts from Internet
- The Container Image Must Only Expose Non-Privileged Ports
    - nothing below 1024
- The Container Image Must Be Built With a Process Health Check
- Container Image Creation Must Use TLS 1.2 or Higher for Secure Container Image Registry Pulls
- The Container Image Should Be Built with Minimal Cached Layers
- The Container Image Must Be Created Without Confidential Data in the Build Files
    - store secrets in variables, configmaps, or an authentication management service
- The Container Images Must Be Created from Signed Base Images
- The Container Image Must Be Created with Verified Packages
- The Container Image Must Be Created with Only Essential Capabilities
- The Container Image Must Only Enable Ports Used for the Service Being Implemented
- The Container Image Must Be Built from a DoD Approved Base Image
- Container Images No Longer in Use Due to Updated Versions Must Be Removed (from the registry)
- The Container Image Must Implement Any STIG or SRG Guidance Relevant to the Container Service
- The Container Image Must Be Created from a Trusted and Approved Source
- The Container Image Must Be Clear of Embedded Credentials

## CONTAINER DEPLOYMENT
(all below instructions are used with `docker run`)
- A Container Must Not Mount the Container Platform's Registry Endpoint
    - DO NOT mount to UNIX socket `-v /var/run/docker.sock:/var/run/docker.sock`
- A Container Must Be Limited in Available System Calls
    - do NOT use `--privileged`
    - `--cap-drop all`
    - `--cap-drop SETUID --cap-drop SETGID`
    - use this to prevent privilege escalations `--security-opt=no-new-privileges`
- Enable PIDs Control Groups to Limit and Account for Container Resource Usage
- Sensitive Directories on the Host System Must Not Be Mounted by Containers
    - `/var/run/docker.sock`, `/proc`, `/dev`, `/etc/`, `/usr`
- The Container Should Have Resource Limits Set
    - max memory `-m`
    - max cpus `--cpus=<value>`
    - max # restarts `--restart=on-failure:<number_of_restarts>`
    - max # file descriptors `--ulimit nofile=<number>`
    - max # processes `--ulimit nproc=<number>`
- The Container Should Have Resource Request Set
- The Container root filesystem Must Be Mounted as Read-Only
    - `--read-only`
    - depending on the application, this requirement may not be possible to implement
    - additionally, this will set volumes to read-only `-v $(pwd):/secrets:ro`
- The Container Must Have a Liveness Probe
- The Container Must Have a Readiness Probe
- A Container Must Not Have Access to Operating System Kernel Namespaces
- The Container Should Be Given Label Selectors to Help Define Container Execution Location and Type

## OTHER BEST PRACTICES
- Whenever possible, emit logs to stdout
- It is imperative for any application/container that has User access (with a backend for example) to leverage CAC integration through SAML for example. If CAC integration is not possible, Multi Factor Authentication must be used but that will require a waiver and is NOT recommended.
- Ensure that TLS and secure certificates are utilized to connect with other services/containers.
