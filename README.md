# docker-images

[![Docker](https://img.shields.io/github/workflow/status/dioptra-io/docker-images/Docker?logo=github&label=docker)](https://github.com/dioptra-io/docker-images/actions/workflows/docker.yml)

This repository contains Docker images for various external utilities.

To add an image name `my-image`:
- Create `my-image/Dockerfile`
- Add `my-image` to the build matrix of the [`docker.yml`](.github/workflows/docker.yml) workflow
- The image will be available at `ghcr.io/dioptra-io/docker-images/my-image:main`
