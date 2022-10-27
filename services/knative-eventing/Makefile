# Copyright 2022 VMware. All Rights Reserved.
# SPDX-License-Identifier: Apache-2.0

REQUIRED_BINARIES := ytt vendir

all: sync

check-carvel:
	$(foreach exec,$(REQUIRED_BINARIES),\
		$(if $(shell which $(exec)),,$(error "'$(exec)' not found. Carvel toolset is required. See instructions at https://carvel.dev/#install")))

sync: check-carvel
	vendir sync && \
	rm output/*.yaml && \
	ytt -f overlays/remove-config-sugar.yaml -f overlays/remove-config-tracing.yaml -f overlays/remove-config-observability.yaml -f upstream/eventing.yaml > output/eventing.yaml && \
	ytt -f overlays/add-namespace.yaml -f overlays/remove-seccompprofile.yaml -f upstream/event-display.yaml > output/event-display.yaml && \
	ytt -f overlays/config-observability.yaml > output/config-observability.yaml && \
	ytt -f overlays/config-tracing.yaml > output/config-tracing.yaml && \
	ytt -f overlays/config-sugar.yaml > output/config-sugar.yaml
