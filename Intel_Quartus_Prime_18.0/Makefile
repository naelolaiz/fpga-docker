TOOL=quartus18

URL=https://download.intel.com/akdlm/software/acdsinst/18.0std/614/ib_tar/Quartus-lite-18.0.0.614-linux.tar
INSTALLER=Quartus-lite-18.0.0.614-linux.tar
CHECKSUM=D8FB4FF23E0EFF722C6BFE65EA2BD265

FILES=$(INSTALLER) Dockerfile

all: container-image-built

${INSTALLER}:
	curl -O $(URL)
	@SUM=$$(md5sum $(INSTALLER) | awk '{print toupper($$1)}'); \
	  if [ "$$SUM" != "$(CHECKSUM)" ]; then echo "Checksum doesn't match."; exit 1; fi

container-image-built: $(FILES)
	@podman rmi fpga:$(TOOL) &>/dev/null || true
	@podman build -t fpga:$(TOOL) --build-arg "TOOL=$(TOOL)" . 
	@podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.temp=1" -q)
	@touch $@

.PHONY: clean
clean:
	-podman rmi $(TAG) >&/dev/null || true
	-podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.temp=1" -q)
	-podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.tool=$(TOOL)" -q)
	-rm -f container-image-built
