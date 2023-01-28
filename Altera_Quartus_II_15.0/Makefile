TOOL=quartus15

URL=https://download.altera.com/akdlm/software/acdsinst/15.0.2/153/ib_tar/Quartus-web-15.0.2.153-linux.tar
INSTALLER=Quartus-web-15.0.2.153-linux.tar
INSTALLER_CHECKSUM=B087BFA443C97FCC9A8904FB73691AD1

FILES=$(INSTALLER) Dockerfile

all: container-image-built

${INSTALLER}:
	curl -O $(URL)
	@SUM=$$(md5sum $(INSTALLER) | awk '{print toupper($$1)}'); \
        if [ "$$SUM" != "$(INSTALLER_CHECKSUM)" ]; then echo "Installer checksum doesn't match."; exit 1; fi
	
container-image-built: $(FILES)
	@podman rmi fpga:$(TOOL) &>/dev/null || true
	@podman build -t fpga:$(TOOL) --build-arg "TOOL=$(TOOL)" . 
	podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.temp" -q)
	@touch $@

.PHONY: clean
clean:
	podman rmi fpga:$(TOOL) >&/dev/null || true
	podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.tool=$(TOOL)" -q)
	podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.temp" -q)
	rm -f container-image-built

