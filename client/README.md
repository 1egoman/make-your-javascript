# Building a client-side app

## The build system
This is a `Makefile` for building javascript code for the frontend. After setting a few environment
variables in the Makefile, (namely, `TARGET` and `ENTRY`), running `make` will self-bootstrap
Browserify and Babel, build a bundle, and output it to the path `TARGET`.

## Tasks
- `make`: Create a new build of the `TARGET`. Before bundling, each module is transformed through
  Babel and packaged with Browserify. On first run of `make`, all dependencies that are required for
  the build process to complete are installed as devDependencies.
- `make watch`: Similar to `make` above, but runs in the background and listens for changes. When
  changes are detected, the bundle is live-updated. Each module is still transformed through Babel,
  but Watchify is used to listen and live-update bundles.
- `make clean`: Remove the bundle from disk.
- `make clean-all`: Remove the bundle, all dependencies, and configuration from disk.

## Build Options
- `TARGET`: Where to output the final build; by default, it's `bundle.js`.
- `ENTRY`: The path to your app's entry point. This is the "main file" that imports in all of your
  other code.
- `TRANSFORMS`: A space-seperated list of babel presets to add to the build process. Two of the most
  common are `es2015` (for es2015 syntax) and `react` (for jsx transpilation).
- `FLAGS`: Any additional flags to pass to Browserify or Watchify when building.
