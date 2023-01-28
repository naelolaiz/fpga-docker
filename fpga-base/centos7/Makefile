all: container-image-built

container-image-built: Dockerfile
	@podman rmi fpga-base:centos7 &>/dev/null || true
	podman build -t fpga-base:centos7 .
	@touch $@

.PHONY: clean
clean:
	podman rmi fpga-base:centos7 >&/dev/null || true
	rm -f container-image-built
