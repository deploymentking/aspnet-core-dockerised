# --------------------------------------------------------------------
# Copyright (c) 2019 LINKIT, The Netherlands. All Rights Reserved.
# Author(s): Anthony Potappel
# 
# This software may be modified and distributed under the terms of the
# MIT license. See the LICENSE file for details.
# --------------------------------------------------------------------

# If you see pwd_unknown showing up, this is why. Re-calibrate your system.
PWD ?= pwd_unknown

# PROJECT_NAME defaults to name of the current directory.
# should not to be changed if you follow GitOps operating procedures.
PROJECT_NAME = $(notdir $(PWD))

# Note. If you change this, you also need to update docker-compose.yml.
# only useful in a setting with multiple services/ makefiles.
DOCKER_REGISTRY := commonwebacr.azurecr.io
DOCKER_IMAGE := oxford/dot-net-core-k8s-test

THIS_FILE := $(lastword $(MAKEFILE_LIST))
TAG_ARGUMENTS ?= $(tag)

# export such that its passed to shell functions for Docker to pick up.
export PROJECT_NAME
export HOST_USER
export HOST_UID

# all our targets are phony (no files to check).
.PHONY: shell help build rebuild service login test clean prune

# suppress makes own output
#.SILENT:

# Regular Makefile help
# help is the first target. So instead of: make help, we can type: make.
help:
	@echo ''
	@echo 'Usage: make [TARGET] [EXTRA_ARGUMENTS]'
	@echo 'Targets:'
	@echo '  build    	build docker --image-- : $(DOCKER_REGISTRY)/$(DOCKER_IMAGE)'
	@echo '  deploy  	push docker --image-- : $(DOCKER_REGISTRY)/$(DOCKER_IMAGE)'
	@echo ''
	@echo 'Extra arguments:'
	@echo 'tag=:	make build tag="whoami"'


# more examples: make shell tag="test && env", make shell tag="echo hello container space".
# leave the double quotes to prevent commands overflowing in makefile (things like && would break)
# special chars: '',"",|,&&,||,*,^,[], should all work. Except "$" and "`", if someone knows how, please let me know!).
# escaping (\) does work on most chars, except double quotes (if someone knows how, please let me know)
# i.e. works on most cases. For everything else perhaps more useful to upload a script and execute that.
build:
ifeq ($(TAG_ARGUMENTS),)
	docker build --tag $(DOCKER_REGISTRY)/$(DOCKER_IMAGE) .
else
	docker build --tag $(DOCKER_REGISTRY)/$(DOCKER_IMAGE):$(TAG_ARGUMENTS) .
endif

push:
ifeq ($(TAG_ARGUMENTS),)
	az acr login --name commonwebacr && docker push $(DOCKER_REGISTRY)/$(DOCKER_IMAGE):latest
else
	az acr login --name commonwebacr && docker push $(DOCKER_REGISTRY)/$(DOCKER_IMAGE):$(TAG_ARGUMENTS)
endif

apply:
	kubectl apply -f deployment.yaml
