all: container-image-built
TAG=centos6

container-image-built: Dockerfile fpga
	@podman rmi fpga-base:$(TAG) &>/dev/null || true
	podman build -t fpga-base:$(TAG) .
	@touch $@

.PHONY: clean
clean:
	podman rmi fpga-base:$(TAG) >&/dev/null || true
	rm -f container-image-built
