# clang-format-python Docker Image

A minimal Ubuntu-based Docker image containing:
- `clang-format-14`
- `python3`

Built to be used for **consistent C++ code formatting** inside CI and developer environments.

This Docker image is **specifically used to format and validate** the codebase of the [FastLanes](https://github.com/cwida/FastLanes) project.

---

## Usage

Pull the image:

```bash
docker pull ghcr.io/azimafroozeh/clang-format-python:14
