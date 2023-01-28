TAG=ubuntu-bionic

all: container-image-built

container-image-built: Dockerfile
	@podman rmi fpga-base:$(TAG) &>/dev/null || true
	podman build -t fpga-base:$(TAG) .
	@touch $@

.PHONY: clean
clean:
	podman rmi fpga-base:$(TAG) >&/dev/null || true
	rm -f container-image-built
