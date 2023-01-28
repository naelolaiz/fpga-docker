TOOL=quartus14

INSTALLER=Quartus-web-14.0.0.200-linux.tar
UPDATES=AOCLSetup-14.0.2.209-linux.run ModelSimSetup-14.0.2.209-linux.run QuartusSetup-14.0.2.209-linux.run SoCEDSSetup-14.0.2.274-linux.run
FILES=$(INSTALLERS) $(UPDATES) Dockerfile

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

