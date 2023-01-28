TOOL=quartus141

INSTALLERS=Quartus-web-14.1.0.186-linux.tar QuartusSetup-14.1.1.190-linux.run AOCLSetup-14.1.1.190-linux.run
FILES=$(INSTALLERS) Dockerfile

all: container-image-built

.PHONY: download
download: installers.txt installers.md5
	@echo "Downloading files..."
	wget -nc -i installers.txt
	@echo "Comparing checksums..."
	md5sum -c installers.md5

container-image-built: download $(FILES)
	@podman rmi fpga:$(TOOL) &>/dev/null || true
	@podman build --rm -t fpga:$(TOOL) --build-arg "TOOL=$(TOOL)" .
	@podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.temp" -q)
	@touch $@

.PHONY: clean
clean:
	podman rmi fpga:$(TOOL) >&/dev/null || true
	podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.tool=$(TOOL)" -q)
	podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.temp" -q)
	rm -f container-image-built
