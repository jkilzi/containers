CONTAINER_RUNTIME := $(shell command -v podman 2> /dev/null || echo docker)
IMAGE := quay.io/jkilzi/openapi-generator-cli
TAG := 7.18.0
ARGS :=

.DEFAULT_GOAL := help

.PHONY: generate
generate:
	$(MAKE) run ARGS="generate $(ARGS)"

.PHONY: run
run:
	$(CONTAINER_RUNTIME) run --name openapi-generator-cli --rm -v $(PWD):/opt/app-root/src $(IMAGE):$(TAG) $(ARGS)

.PHONY: clean
clean:
	$(CONTAINER_RUNTIME) rmi $(IMAGE):$(TAG)

.PHONY: build
build:
	$(CONTAINER_RUNTIME) build --platform linux/amd64 -t $(IMAGE):$(TAG) -f Containerfile

.PHONY: push
push:
	$(CONTAINER_RUNTIME) push $(IMAGE):$(TAG)

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
	@echo "Container runtime: $(CONTAINER_RUNTIME)"
