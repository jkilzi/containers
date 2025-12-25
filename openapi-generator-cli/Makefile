IMAGE := quay.io/jkilzi/openapi-generator-cli
TAG := 7.18.0
ARGS :=

.DEFAULT_GOAL := help

.PHONY: generate
generate:
	$(MAKE) run ARGS="generate $(ARGS)"

.PHONY: run
run:
	podman run --name openapi-generator-cli --rm -v $(PWD):/opt/app-root/src $(IMAGE):$(TAG) $(ARGS)

.PHONY: clean
clean:
	podman rmi $(IMAGE):$(TAG)

# Based on https://developers.redhat.com/articles/2023/11/03/how-build-multi-architecture-container-images
.PHONY: create-manifest
create-manifest:
	podman manifest create $(IMAGE):$(TAG)

.PHONY: build
build: create-manifest
	podman build --platform linux/amd64,linux/arm64 --manifest $(IMAGE):$(TAG) -f Containerfile .

.PHONY: push
push:
	podman manifest push $(IMAGE):$(TAG)

.PHONY: help
help:
	@echo "OpenAPI Generator CLI Makefile"
	@echo ""
	@echo "Available targets:"
	@echo "  run                     Run the OpenAPI Generator CLI container"
	@echo "  generate                Run openapi-generator-cli (pass ARGS for extra arguments)"
	@echo "  clean                   Remove the OpenAPI Generator CLI container image"
	@echo "  build                   Build the OpenAPI Generator CLI container image"
	@echo "  push                    Push the OpenAPI Generator CLI container image to the registry"
	@echo "  help                    Show this help message"
	@echo ""
