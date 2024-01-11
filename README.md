# `ghcr.io/amp-buildpacks/leo`

The Leo Cloud Native Buildpack provides a set of collaborating buildpacks that
enable the building of a Leo-based application.

## Included Buildpacks

- [`amp-buildpacks/leo-dist`](https://github.com/amp-buildpacks/leo-dist)
- [`amp-buildpacks/aleo`](https://github.com/amp-buildpacks/aleo)
- [`paketo-buildpacks/procfile`](https://github.com/paketo-buildpacks/procfile)

## tl;dr

You can build your app with two steps:

1. Run `pack build <image-name> -b ghcr.io/amp-buildpacks/leo`. An image will
   be generated that you can run.
2. Run the image with `docker run -it <image-name>`.

By default, the Leo buildpack will add process types for all of the binary
projects in your workspace. You may optionally add a
[`Procfile`](https://paketo.io/docs/howto/configuration/#procfiles) though if
you need to customize the start command for your container.

## What's Included

### A Meta Buildpack

Included in this repo is a `buildpack.toml` that defines the Leo meta
buildpack. It allows you to reference all of the Leo related CNBs in one
convenient way & know that the order is set correctly.

This is available through a pre-built docker image on [Amphitheatre Buildpacks
Packages](https://github.com/orgs/amp-buildpacks/packages).

To use: `pack build <image-name> -b ghcr.io/amp-buildpacks/leo`

### A Builder

In this repo is a sample `example-builder.toml` that you can use to create your
own builder. We do not publish a builder, so if you'd like to use this route you
must first create the builder.

To create the builder, just run

```shell
pack builder create <published-to>/leo-builder --config example-builder.toml
```

For example

```shell
pack builder create amp-buildpacks/leo-builder --config example-builder.toml
```

You can then build an app with it using 

```shell
pack build <image-name> --builder <published-to>/leo-builder
```

The builder is configure to use the base build and run images, which is a
reasonable mix of size and functionality. You may change the build and run
images to use the full stack, which has a lot more libraries and tools installed
but is quite a bit larger, or you can use the tiny build and run images which
presents a very small container but doesn't even contain a shell, which can make
debugging difficult.

To switch, swap the stack that you'd like to use in the builder file.

Example full stack:

```toml
[stack]
  id = "io.buildpacks.stacks.jammy"
  build-image = "docker.io/paketobuildpacks/build-jammy-base:latest"
  run-image = "docker.io/paketobuildpacks/run-jammy-base:latest"
```

Example tiny stack:

```toml
[stack]
  id = "io.buildpacks.stacks.jammy.tiny"
  build-image = "docker.io/paketobuildpacks/build-jammy-tiny"
  run-image = "docker.io/paketobuildpacks/run-jammy-tiny"
```

## Contributing

If anything feels off, or if you feel that some functionality is missing, please
check out the [contributing
page](https://docs.amphitheatre.app/contributing/). There you will find
instructions for sharing your feedback, building the tool locally, and
submitting pull requests to the project.

## License

Copyright (c) The Amphitheatre Authors. All rights reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

## Credits

Heavily inspired by https://github.com/paketo-community/rust
