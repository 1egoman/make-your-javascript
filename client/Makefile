# vim: set autoindent noexpandtab tabstop=4 shiftwidth=4 syntax=Make :

# ------------------------------------------------------------------------------
# CONFIGURABLE BUILD OPTIONS
# ------------------------------------------------------------------------------
TARGET=bundle.js # the output of the compiled code will go here.
ENTRY=src/app.js # where to fetch input code that will be bundled

# Which babel transforms to use when bundling
TRANSFORMS := es2015

# Additional flags to pass to either browserify or watchify when compiling.
FLAGS=

BROWSERIFY=./node_modules/.bin/browserify
WATCHIFY=./node_modules/.bin/watchify
NPM=npm

# Enable sourcemap with "make sourcemap=1"
ifdef sourcemap
	FLAGS += --debug
endif

# Add any babel transforms
ifdef TRANSFORMS
	FLAGS += -t [babelify]
endif

all: build

# Setup the build environment by installing all dependencies, and also
# install browserify / watchify if they are not installed already.
node_modules:
	$(NPM) install
$(BROWSERIFY):
	$(NPM) install --save-dev browserify
$(WATCHIFY):
	$(NPM) install --save-dev watchify

# if babel transforms were specified (and they aren't already installed), install them too.

# This is the collective target for all transform-related stuff.
transforms: node_modules/babelify $(addprefix node_modules/babel-preset-,$(TRANSFORMS)) .babelrc

# install babelify
node_modules/babelify:
	npm install --save-dev babelify

# install each transform
node_modules/babel-preset-%:
	npm install --save-dev $(@F)

# create a babelrc with all the babel transforms specified above.
# TODO: the below sed hack is horrible. Could and should be made a lot cleaner.
.babelrc:
	@echo '{"presets": [$(shell echo '$(foreach transform,$(TRANSFORMS),"$(transform)",)' | sed 's/,$$//')]}' > .babelrc




.PHONY: build
build: $(ENTRY) node_modules $(BROWSERIFY) transforms
	$(BROWSERIFY) $(FLAGS) -o $(TARGET) -- $<

.PHONY: watch
watch: $(ENTRY) node_modules $(WATCHIFY) transforms
	$(WATCHIFY) --verbose $(FLAGS) -o $(TARGET) -- $<


# The below two targets make clean builds.
# clean: just nuke the target.
# clean-all: nuke the target and all its dependencies.

.PHONY: clean
clean:
	rm -rf $(TARGET)

.PHONY: clean-all
clean-all: clean
	rm -rf node_modules/ .babelrc
