# <img alt="" width="48" align="top"  src="https://raw.githubusercontent.com/rust-skia/rust-skia/master/artwork/rust-skia-icon_512x512.png"/> Safe Rust bindings to the [Skia Graphics Library](https://skia.org/).

This packages contains safe Rust wrappers for Skia and uses [skia-bindings](https://crates.io/crates/skia-bindings) to build and interface with the Skia C++ library.

For information about the supported build targets and how to run the examples, please visit the [github page of the rust-skia project](https://github.com/rust-skia/rust-skia).

## Documentation

Function level documentation is [not yet](https://github.com/rust-skia/rust-skia/issues/23) available. To get started, take a look at the [Rust examples](https://github.com/rust-skia/rust-skia/tree/master/skia-safe/examples/skia-org) or the [Skia documentation](https://skia.org). 

## Bindings & Wrappers

Skia-safe wraps most parts of the public Skia C++ APIs:

- [x] Vector Geometry: Matrix, Rect, Point, Size, etc.
- [x] Most drawing related classes and functions: Surface, Canvas, Paint, Path.
- [x] Effects and Shaders.
- [x] Utility classes we think are useful.
- [x] PDF & SVG rendering
- [ ] Skia Modules
  - [x] Text shaping with [Harfbuzz](https://www.freedesktop.org/wiki/Software/HarfBuzz/) and [ICU](http://site.icu-project.org/home).
  - [x] Text layout (skparagraph)
  - [ ] Animation via [Skottie](https://skia.org/user/modules/skottie)
- [x] GPU Backends
  - [x] Vulkan
  - [x] OpenGL
  - [x] Metal

Wrappers for functions that take callbacks and virtual classes are not supported right now. While we think they should be wrapped, the use cases related seem to be rather special, so we postponed that for now.

## Features

Skia-safe supports the following features that can be configured [via cargo](https://doc.rust-lang.org/cargo/reference/manifest.html#the-features-section):

### `gl`

Platform support for OpenGL or OpenGL ES can be enabled by adding the feature `gl`. Since version `0.25.0`, rust-skia is configured by default to enable CPU rendering only. Before that, OpenGL support was included in every feature configuration. To render the examples with OpenGL, use

```bash
(cd skia-org && cargo run --features gl [OUTPUT_DIR] --driver opengl)
```

### `vulkan`

Vulkan support can be enabled by adding the feature `vulkan`. To render the examples with Vulkan, use

```bash
(cd skia-org && cargo run --features vulkan [OUTPUT_DIR] --driver vulkan)
```

Note that Vulkan drivers need to be available. On Windows, they are most likely available already, on Linux [this article on linuxconfig.org](<https://linuxconfig.org/install-and-test-vulkan-on-linux>) might get you started, and on macOS with Metal support, [install the Vulkan SDK](<https://vulkan.lunarg.com/sdk/home>) for Mac and configure MoltenVK by setting the `DYLD_LIBRARY_PATH`, `VK_LAYER_PATH`, and `VK_ICD_FILENAMES` environment variables as described in `Documentation/getting_started_macos.html`.

### `metal`

Support for Metal on macOS and iOS targets can be enabled by adding the feature `metal`.

### `textlayout`

The Cargo feature `textlayout` enables text shaping with Harfbuzz and ICU by providing bindings to the Skia modules skshaper and skparagraph. 

The skshaper module can be accessed through `skia_safe::Shaper` and the Rust bindings for skparagraph are in the `skia_safe::textlayout` module. 

On **Windows**, the file `icudtl.dat` must be available in your executable's directory. To provide the data file, either copy it from the build's output directory (shown when skia-bindings is compiled with `cargo build -vv | grep "ninja: Entering directory"`), or - if your executable directory is writable - invoke the function `skia_safe::icu::init()` before using the `skia_safe::Shaper` object or the `skia_safe::textlayout` module. 

Simple examples of the skshaper and skparagraph module bindings can be found [in the skia-org example command line application](https://github.com/rust-skia/rust-skia/blob/master/skia-safe/examples/skia-org).



