TOOL=quartus17

URL=https://download.altera.com/akdlm/software/acdsinst/17.0std.2/602/ib_tar/Quartus-lite-17.0.2.602-linux.tar
INSTALLER=Quartus-lite-17.0.2.602-linux.tar
CHECKSUM=6FB433BF0A999C5B2BF3C0B43FE664A8

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
