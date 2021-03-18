## Current hardening procedure
1. STIG the base image
2. STIG the application
3. Clean container scan (Anchore for vulnerabilities, OpenSCAP for compliance)

OR

1. Use a STIG'd / hardened application image from Ironbank


## Software in supply chain that is assumed to be hardened
- container runtime (Docker Enterprise, Podman)
- container platform (OpenShift)
- virtual machine / bare metal
- networking rules at every level


## References
- https://software.af.mil/dsop/documents/
- https://cloudsecdocs.com
- https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html
- https://github.com/OWASP/Docker-Security
