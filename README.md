# `clang-format-python` Docker Image
**Multi-arch (`amd64` + `arm64`) · clang-format 14 · Python 3**

[![Docker - latest](https://img.shields.io/badge/ghcr.io%2Fazimafroozeh-clang--format--python-0A7ACA?logo=docker&label=latest)](https://github.com/users/azimafroozeh/packages/container/package/clang-format-python)
[![Build & Push CI](https://github.com/azimafroozeh/clang-format-python/actions/workflows/docker-push.yml/badge.svg)](https://github.com/azimafroozeh/clang-format-python/actions)

A **minimal Ubuntu 22.04** container bundling the exact tools most C/C++ projects need for
reproducible formatting and small helper scripts:

| Tool            | Version (frozen)          | Path in container            |
|-----------------|---------------------------|------------------------------|
| **clang-format**| `14.0.x` (Ubuntu 22.04 LTS) | `/usr/bin/clang-format`      |
| **Python**      | `3.10.x`                  | `/usr/bin/python3`           |

* Multi-arch manifest — works on **GitHub Actions runners** (`linux/amd64`),  
  Apple Silicon & other Arm machines (`linux/arm64`).
* Primary user: the [FastLanes](https://github.com/cwida/FastLanes) file format
  project, but the image is generic for any code-base that wants a **stable clang-format 14**
  plus Python scripting inside CI.

---

## Quick start

```bash
# 1 · Pull (Docker / Podman)
docker pull ghcr.io/azimafroozeh/clang-format-python:14   # or :latest

# 2 · Format a file in-place
docker run --rm \
  -v "$(pwd)":/work -w /work \
  ghcr.io/azimafroozeh/clang-format-python:14 \
  clang-format -i src/foo.cpp
```

> **Repo-scoped tag** (same bits, different namespace):  
> `ghcr.io/azimafroozeh/clang-format-python/clang-format-python:14`

---

## Typical workflows

<details>
<summary><strong>Make targets</strong></summary>

```makefile
IMAGE = ghcr.io/azimafroozeh/clang-format-python:14

format:
	docker run --rm -v "$(PWD)":/code -w /code $(IMAGE) bash -c "\
	  python3 scripts/run-clang-format.py -r include src -i"

format-check:
	docker run --rm -v "$(PWD)":/code -w /code $(IMAGE) bash -c "\
	  python3 scripts/run-clang-format.py -r include src"
```
</details>

<details>
<summary><strong>GitHub Actions check</strong></summary>

```yaml
jobs:
  clang-format:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run clang-format check
        run: |
          docker run --rm -v "$PWD":/repo -w /repo \
            ghcr.io/azimafroozeh/clang-format-python:14 \
            python3 scripts/run-clang-format.py -r include src
```
</details>

---

## Image details

* **Base**: `ubuntu:22.04`
* APT cache removed → image size ~ 150 MB
* Symlink `/usr/bin/clang-format → clang-format-14`
* Built & published by [docker-push.yml](.github/workflows/docker-push.yml)

### Supported tags

| Tag | Purpose |
|-----|---------|
| `14`, `latest` | identical; rebuilt on every push to `main` |

---

## Building locally

```bash
# one-off: create a multi-arch builder
docker buildx create --use --name multiarch || true

# build & load for the host arch only
docker buildx build -t clang-format-python:dev --load .

# multi-arch push (amd64 + arm64)
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t ghcr.io/azimafroozeh/clang-format-python:14 \
  -t ghcr.io/azimafroozeh/clang-format-python:latest \
  --push .
```

---

## Contributing

1. Edit **`Dockerfile`** (e.g. bump LLVM release).  
2. Push to `main` – CI rebuilds & pushes multi-arch tags.  
3. PRs & issues welcome!

---

## License

Code in this repo is MIT-licensed.  
`clang-format` binary remains under the LLVM license.


