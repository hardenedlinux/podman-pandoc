EXEC = pandoc
DOCNAME=demo
SECCOMP_PROFILE=$(shell pwd)/$(DOCNAME)_seccomp.json
CONTAINER_USER=pan

build: help

.PHONY: html
html:
	podman run --rm -it \
	  --volume "$(shell pwd):/home/$(CONTAINER_USER)" \
	  --userns keep-id:uid=$(shell id -u),gid=$(shell id -g) \
	  localhost/pandoc:latest \
	  $(EXEC) -f markdown -t html $(DOCNAME).md -o $(DOCNAME).html

.PHONY: gen
gen:
	sudo podman run --rm -it \
	  --root ${HOME}/.local/share/containers/storage/ \
      --annotation io.containers.trace-syscall="of:$(SECCOMP_PROFILE)" \
	  --volume "$(shell pwd):/home/$(CONTAINER_USER)" \
	  --userns keep-id:uid=$(shell id -u),gid=$(shell id -g) \
	  localhost/pandoc:latest \
	    $(EXEC) -f markdown -t html $(DOCNAME).md -o $(DOCNAME).html
	
	sudo chown -R $(shell id -un):$(shell id -gn) \
	  $(SECCOMP_PROFILE) \
	  ${HOME}/.local/share/containers/storage/ \
	  /tmp/storage-run-$(shell id -u)/containers/overlay-layers/mountpoints.json

.PHONY: lint
lint:
	python3 -mjson.tool $(SECCOMP_PROFILE) $(SECCOMP_PROFILE)

.PHONY: run
run:
	podman run --rm -it \
	  --security-opt seccomp=$(SECCOMP_PROFILE) \
	  --volume "$(shell pwd):/home/$(CONTAINER_USER)" \
	  --userns keep-id:uid=$(shell id -u),gid=$(shell id -g) \
	  localhost/pandoc:latest \
	    $(EXEC) -f markdown -t html $(DOCNAME).md -o $(DOCNAME).html

.PHONY: clean
clean:
		rm -f $(DOCNAME).html

.PHONY: view
view:
		cat $(DOCNAME).html

help:
	@echo 'Convert markdown to html using containerized (podman) pandoc  '
	@echo '                                                              '
	@echo 'Usage:                                                        '
	@echo '                                                              '
	@echo 'make help   The help                                              '
	@echo 'make gen    Generate the seccomp profile                      '
	@echo 'make run    Run the containerized pandoc using the generated  '
	@echo '            seccomp profile                                   '
	@echo 'make lint   Format the generated seccomp profile              '
	@echo 'make html   Run the containerized pandoc without using the    '
	@echo '            generated seccomp profile                         '
	@echo 'make clean  Clean up the html file                            '
	@echo 'make view   Show the contents of the html file                '
	@echo '                                                              '
 

