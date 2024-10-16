# podman-pandoc

A demonstration of container hardening process. Check [Container Hardening Process](https://hardenedlinux.org/blog/2024-10-13-container-hardening-process/) at hardenedlinux.org for the instructions.

### Files included

* `build/Dockerfile` - for building a simple pandoc container for the demonstration.
* `demo/Makefile` - contains commands to create and apply customized Seccomp profile using [oci-seccomp-bpf-hook](https://github.com/containers/oci-seccomp-bpf-hook).
* `default.seccomp.json` - the default Seccomp profile copied from `/usr/share/containers/seccomp.json` for reference.

### Notes

1. [Podman](https://podman.io/) is used in this demonstration as the container platform.
2. `cd demo/; make help` to see how to use the commands.

