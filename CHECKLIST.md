**COMPLIANCE**
- STIG base image
- STIG application
- NIST800-53v4 moderate controls plus FedRAMP+ IL6controls
- NISTSP800-190 and Center for Internet Security (CIS) Docker Benchmark

**IMAGE CREATION**
- The Container Image Must Be Built with the SSH Server Daemon Disabled
- The Container Image Must Be Created to Execute as a Non-Privileged User 
    - no root user (UID 0)
    - In Dockerfile `RUN groupadd -r user && useradd -r -g user user` and `USER user`
- The Container Image Must Have Permissions Removed from Executables that Allow a User to Execute Software at Higher Privileges
    - `docker run --cap-drop SETUID --cap-drop SETGID <image>`
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

**CONTAINER DEPLOYMENT**
- A Container Must Not Mount the Container Platform's Registry Endpoint
    - DON'T mount to UNIX socket `docker run -it -v /var/run/docker.sock:/var/run/docker.sock ubuntu /bin/bash`
- A Container Must Be Limited in Available System Calls
- Enable PIDs Control Groups to Limit and Account for Container Resource Usage
- Sensitive Directories on the Host System Must Not Be Mounted by Containers
    - `/var/run/docker.sock`, `/proc`, `/dev`, `/etc/`, `/usr`
- The Container Should Have Resource Limits Set
- The Container Should Have Resource Request Set
- The Container root filesystem Must Be Mounted as Read-Only
    - `docker run --read-only <image>`
    - depending on the application, this requirement may not be possible to implement
    - additionally, this will set volumes to read-only `docker run -v $(pwd):/secrets:ro`
- The Container Must Have a Liveness Probe
- The Container Must Have a Readiness Probe
- A Container Must Not Have Access to Operating System Kernel Namespaces (on by default)
- The Container Should Be Given Label Selectors to Help Define Container Execution Location and Type

**OTHER BEST PRACTICES**
- Whenever possible, emit logs to stdout
- It is imperative for any application/container that has User access (with a backend for example) to leverage CAC integration through SAML for example. If CAC integration is not possible, Multi Factor Authentication must be used but that will require a waiver and is NOT recommended.
- Ensure that TLS and secure certificates are utilized to connect with other services/containers.
