export PYODIDE_ROOT=$(abspath ..)
PYODIDE_LIBRARIES=$(abspath ./.artifacts)
include ../Makefile.envs

ifeq ($(strip $(PYODIDE_PACKAGES)),)
else
	ONLY_PACKAGES=--only "$(PYODIDE_PACKAGES)"
endif

all: .artifacts/bin/pyodide-build
	PYTHONPATH="$(PYODIDE_LIBRARIES)/lib/python:$(PYODIDE_ROOT)/pyodide-build/" pyodide-build buildall . ../build \
		--target=$(TARGETPYTHONROOT) $(ONLY_PACKAGES) --install-dir $(PYODIDE_LIBRARIES) --n-jobs $${PYODIDE_JOBS:-4}

.artifacts/bin/pyodide-build: ../pyodide-build/pyodide_build/**
	mkdir -p $(PYODIDE_LIBRARIES)
	python3 -m pip install -e ../pyodide-build --no-deps --prefix $(PYODIDE_LIBRARIES)

update-all:
	for pkg in $$(find . -maxdepth 1 -type d -exec basename {} \; | tail -n +2); do \
		pyodide-build mkpkg "$${pkg}" --update; \
	done

clean:
	rm -rf ./*/build ./*/build.log
	rm -rf ./.artifacts
