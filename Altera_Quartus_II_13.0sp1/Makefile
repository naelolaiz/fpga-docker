TOOL=quartus13
INSTALLER=Quartus-web-13.0.1.232-linux.tar

all: container-image-built

$(INSTALLER):
	curl -O http://download.altera.com/akdlm/software/acdsinst/13.0sp1/232/ib_tar/Quartus-web-13.0.1.232-linux.tar

container-image-built: $(INSTALLER) Dockerfile
	@podman rmi fpga:$(TOOL) &>/dev/null || true
	@podman build -t fpga:$(TOOL) --build-arg "TOOL=$(TOOL)" .
	@podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.temp" -q)
	@touch $@

.PHONY: clean
clean:
	podman rmi fpga:$(TOOL) >&/dev/null || true
	podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.tool=$(TOOL)" -q)
	podman rmi $$(podman images --filter "label=com.halfmanhalftaco.fpga.temp" -q)
	rm -f container-image-built
