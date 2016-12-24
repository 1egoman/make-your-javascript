# vim: set autoindent noexpandtab tabstop=4 shiftwidth=4 syntax=Make :

JS_SRC=src/app.js # 
JS_TRANSFORMS := es2015 # Which babel transforms to use when bundling
JS_MINIFY=ON # should code be minified?
JS_SOURCEMAP=ON # should a sourcemap be incorporated into the source?
#JS_WATCH=ON # should watch for changes and rebuild
JS_FLAGS= # any more flags to add to browserify

# Tells make how to implicitly handle javascript files with the above configration
include makefiles/javascript

SCSS_SRC=sass/styles.scss #
SCSS_MINIFY=ON

include makefiles/scss


# The below says that for the default rule to pass, we need a `bundle.js`.
# The javascript handler imported above knows how to make javascript files! So it takes over.
.PHONY: all
all: bundle.js bundle.css

# Watch a file, and rebuild it.
.PHONY: watch
watch: 
	@JS_WATCH=ON SCSS_WATCH=ON make all -j 2

.PHONY: clean
clean:
	rm -rf node_modules/ bundle.js bundle.css

# tasks that this pulls in:
# - js-build-dev
# - js-build-release
# - js-watch
# - js-clean
# - js-clean-all
# - js-transforms (private)

# include makefiles/scss
