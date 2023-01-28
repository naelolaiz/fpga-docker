TOOL=quartus16

URL=https://download.intel.com/akdlm/software/acdsinst/16.0.2/222/ib_tar/Quartus-lite-16.0.2.222-linux.tar
INSTALLER=Quartus-lite-16.0.2.222-linux.tar
INSTALLER_CHECKSUM=FE3C46D6AA6B3F08AEFF0666796E12EE

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
	@podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.temp=1" -q)
	@touch $@

.PHONY: clean
clean:
	-podman rmi $(TAG) >&/dev/null
	-podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.temp=1" -q)
	-podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.tool=$(TOOL)" -q)
	-rm -f container-image-built

