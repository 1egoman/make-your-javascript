# vim: set autoindent noexpandtab tabstop=4 shiftwidth=4 syntax=Make :
BUNDLES=bundle.js bundle.css
.PRECIOUS: $(BUNDLES) # maek the above files as the output of the bundling operation


# The below says that for the default rule to pass, we need a `bundle.js`.
# The javascript handler imported above knows how to make javascript files! So it takes over.
.PHONY: all
all: $(BUNDLES)

# Watch a file, and rebuild it.
.PHONY: watch
watch: clean
	@JS_WATCH=ON SCSS_WATCH=ON make all -j 2

.PHONY: clean
clean:
	rm -rf $(BUNDLES)

.PHONY: clean-all
clean-all: clean
	rm -rf node_modules/

# tasks that this pulls in:
# - js-build-dev
# - js-build-release
# - js-watch
# - js-clean
# - js-clean-all
# - js-transforms (private)

# include makefiles/scss


JS_SRC=src/app.js # 
JS_TRANSFORMS := es2015 # Which babel transforms to use when bundling
JS_MINIFY=ON # should code be minified?
JS_SOURCEMAP=ON # should a sourcemap be incorporated into the source?
#JS_WATCH=ON # should watch for changes and rebuild
JS_FLAGS= # any more flags to add to browserify

# Tells make how to implicitly handle javascript files with the above configration
include makefiles/javascript

SCSS_SRC=sass/styles.scss
SCSS_MINIFY=ON
SCSS_SOURCEMAP=ON
#SCSS_WATCH=ON
SCSS_FLAGS=

# Tells make how to implicitly handle scss files with the above configration
include makefiles/scss
