# vim: set autoindent noexpandtab tabstop=4 shiftwidth=4 syntax=Make :

# ------------------------------------------------------------------------------
# CONFIGURABLE BUILD OPTIONS
# ------------------------------------------------------------------------------
TARGET=bundle.js # the output of the compiled code will go here.
ENTRY=src/app.js # where to fetch input code that will be bundled

# Which babel transforms to use when bundling
TRANSFORMS := es2015

# When building wiht `make watch`, which port to start the server on
WATCH_PORT=8001

# Additional flags to pass to either browserify or watchify when compiling.
FLAGS=
PRODUCTION_FLAGS=

# ----------------------------------------------------------------------------
# PRODUCTION BUILD SETTINGS
# ----------------------------------------------------------------------------
MINIFY=1 # should code be minified with uglify when building for production?
PACKAGED_LOCATION=bundled/



ifeq (MINIFY, 1)
	PRODUCTION_FLAGS += -g uglifyify
endif

# Enable sourcemap with "make sourcemap=1"
ifdef sourcemap
	FLAGS += --debug
endif

# Babel transforms need a special flag
ifdef TRANSFORMS
	FLAGS += -t [babelify]
endif



BROWSERIFY=./node_modules/.bin/browserify
WATCHIFY=./node_modules/.bin/watchify
NPM=npm

# default to a dev build
all: build-dev

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

# install uglifyify for minification
node_modules/uglifyify:
	npm install --save-dev uglifyify

# create a babelrc with all the babel transforms specified above.
# If a .babelrc already exists, this won't overwrite it (don't worry).
# So feel free to midify the output of this and commit it if you'd like.
# TODO: the below sed hack is horrible. Could and should be made a lot cleaner.
.babelrc:
	@echo '{"presets": [$(shell echo '$(foreach transform,$(TRANSFORMS),"$(transform)",)' | sed 's/,$$//')]}' > .babelrc




# Create a new dev build of the given code.
.PHONY: build-dev
build-dev: $(ENTRY) node_modules $(BROWSERIFY) transforms
	$(BROWSERIFY) $(FLAGS) -o $(TARGET) -- $<

# Create a new production build of the given code.
.PHONY: build-release
build-release: $(ENTRY) node_modules $(BROWSERIFY) transforms $($(MINIFY:1=node_modules/uglifyify):0=)
	$(BROWSERIFY) $(FLAGS) $(PRODUCTION_FLAGS) -o $(TARGET) -- $<

# Watch for changes and make builds on the fly.
.PHONY: watch
watch: $(ENTRY) node_modules $(WATCHIFY) transforms
	$(WATCHIFY) --verbose $(FLAGS) -o $(TARGET) -- $< & \
	python -m SimpleHTTPServer $(WATCH_PORT)


# Targets for building a release
$(PACKAGED_LOCATION):
	mkdir -p $(PACKAGED_LOCATION)

# copy all the things into the packaged location
release-copy:
	mkdir -p $(basename $(PACKAGED_LOCATION))
	cp $(TARGET) $(PACKAGED_LOCATION)/$(TARGET)

release: build-release $(PACKAGED_LOCATION) release-copy


# The below two targets make clean builds.
# clean: just nuke the target.
# clean-all: nuke the target and all its dependencies.

.PHONY: clean
clean:
	rm -rf $(TARGET) $(PACKAGED_LOCATION)

.PHONY: clean-all
clean-all: clean
	rm -rf node_modules/
