TOOL=quartus151

URL=https://download.intel.com/akdlm/software/acdsinst/15.1.2/193/ib_tar/Quartus-lite-15.1.2.193-linux.tar
INSTALLER=Quartus-lite-15.1.2.193-linux.tar
INSTALLER_CHECKSUM=0F5CF26B7BB1837DA350F847DC8C98D8

FILES=$(INSTALLER) Dockerfile

all: container-image-built

${INSTALLER}:
	curl -O $(URL)
	echo "Comparing checksum..."
	@SUM=$$(md5sum $(INSTALLER) | awk '{print toupper($$1)}'); \
        if [ "$$SUM" != "$(INSTALLER_CHECKSUM)" ]; then echo "Installer checksum doesn't match."; exit 1; fi
	
container-image-built: $(FILES)
	@podman rmi fpga:$(TOOL) &>/dev/null || true
	@podman build --rm -t fpga:$(TOOL) --build-arg "TOOL=$(TOOL)" .
	@podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.temp" -q)
	@touch $@

.PHONY: clean
clean:
	podman rmi fpga:$(TOOL) >&/dev/null || true
	@podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.temp" -q)
	@podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.tool=$(TOOL)" -q)
	rm -f container-image-built

