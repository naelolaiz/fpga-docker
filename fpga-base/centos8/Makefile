TAG=fpga-base:centos8

all: container-image-built

container-image-built: Dockerfile
	@podman rmi $(TAG) &>/dev/null || true
	podman build --rm -t $(TAG) .
	@touch $@

.PHONY: clean
clean:
	podman rmi $(TAG) >&/dev/null || true
	rm -f container-image-built
