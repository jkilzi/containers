VCS_REF := $(shell git rev-parse --short HEAD)
BUILD_DATE := $(shell date -u +"%Y-%m-%dT%H:%M:%SZ")
VERSION := 1.0.0

IMAGE := quay.io/jkilzi/openapi-generator-cli
TAG := 7.18.0
ARGS :=

.DEFAULT_GOAL := help

.PHONY: generate
generate:
	$(MAKE) run ARGS="generate $(ARGS)"

.PHONY: run
run:
	podman run --name openapi-generator-cli --rm -v $(PWD):/opt/app-root/src/workspace $(IMAGE):$(TAG) $(ARGS)

.PHONY: clean
clean:
	podman manifest rm $(IMAGE):$(TAG) || true
	@DANGLING_IMAGES=$$(podman images --filter "dangling=true" --noheading --format "{{.ID}}"); \
	if [ -n "$$DANGLING_IMAGES" ]; then \
		podman rmi $$DANGLING_IMAGES; \
	fi

# Based on https://developers.redhat.com/articles/2023/11/03/how-build-multi-architecture-container-images
.PHONY: create-manifest
create-manifest:
	podman manifest create $(IMAGE):$(TAG)

.PHONY: build
build: create-manifest
	podman build \
	--build-arg VCS_REF=$(VCS_REF) \
	--build-arg VERSION=$(VERSION) \
	--build-arg BUILD_DATE=$(BUILD_DATE) \
	--manifest $(IMAGE):$(TAG) \
	--platform linux/amd64,linux/arm64 \
	-f Containerfile .

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
