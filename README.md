# Redis Stack Binaries

This repo provides build rules to compile Redis Stack binaries from source for the [Please](https://please.build) build system.

Additionally, pre-built Redis Stack Binary artifacts are available for download from the GitHub release assets.

## Prebuilt binaries of Redis Stack for Linux `x86_64`

These are intended for use in sandboxed or hermetic build systems (like Please), where installing via `apt` or building from source on the fly isn't ideal. This setup is perfect for SAT frameworks, CI pipelines, or constrained container environments.

## ✅ Available Versions

| Redis Stack Version | Supported OS | C Library | Architecture | Download Link |
|---------------------|--------------|-----------|--------------|---------------|
| 7.4.2               | Linux        | glibc     | x86_64       | [Download](https://github.com/jackmarsh/redis/releases/download/v0.0.1/redis-7.4.2-linux_x86_64.tar.gz) |
| 7.4.2               | Linux (Alpine) | musl (static) | x86_64 | [Download](https://github.com/jackmarsh/redis/releases/download/v0.0.2/redis-7.4.2-linux_x86_64-musl.tar.gz) |

## 📦 What's Included

Each `.tar.gz` archive contains the following Redis Stack binaries:
* `redis-server`
* `redis-cli`
* `redis-sentinel`
* `redis-benchmark`
* `redis-check-aof`
* `redis-check-rdb`

**glibc binaries** are dynamically linked against `glibc` and OpenSSL 3. They are compatible with:
* Ubuntu 22.04+
* Debian 11+
* Fedora 36+
* Arch Linux

**musl binaries** are statically linked and compatible with:
* Alpine Linux (all versions)
* Any musl-based Linux distribution
* Can also run on glibc systems as a fallback

## ✅ Alpine Linux Support

**Alpine-compatible binaries are now available!**

The build system supports both `glibc` (standard Linux) and `musl` (Alpine Linux) builds:

- **glibc builds** (default): Dynamically linked, works on Ubuntu, Debian, Fedora, etc.
- **musl builds**: Statically linked, works on Alpine Linux and other musl-based systems

### Building musl binaries

#### Required Dependencies

Install the following packages on your build system:

```bash
sudo apt-get install musl-tools musl-dev
```

**Note:** Building with TLS support (`BUILD_TLS=yes`) requires static OpenSSL libraries for musl, which are not readily available on Ubuntu/Debian. The musl build currently disables TLS and uses libc malloc instead of jemalloc to ensure a fully static build without external dependencies.

#### Build with Please

```python
redis_stack(
    name = "redis_stack_alpine",
    version = "7.4.2",
    libc = "musl",
    visibility = ["PUBLIC"],
)
```

Then build:
```bash
plz build //binaries/v7.4.2:redis_stack_musl
```

## Basic Usage

### Option 1: Use as a Please Plugin

1. **Add the plugin to your project**

   In `plugins/BUILD`:
   ```python
   plugin_repo(
       name = "redis",
       owner = "jackmarsh",
       revision = "v0.0.1",
   )
   ```

2. **Update your `.plzconfig`**

   Add the following section:
   ```ini
   [Plugin "redis"]
   Target = //plugins:redis
   ```

3. **Use the Redis Stack build rules in your project**

   After setting up the plugin, you can use the `redis_stack` build rule to include Redis Stack binaries in your project. Example:
   ```python
   subinclude("///redis//build_defs:redis")

   redis_stack(
       name = "redis_stack",
       version = "7.4.2",
       visibility = ["PUBLIC"],
   )
   ```

   This will build the specified version of Redis Stack binaries from source.

---

### Option 2: Use Pre-built Artifacts

If you prefer not to build Redis Stack from source, you can directly download pre-built binaries from the [GitHub release assets](https://github.com/jackmarsh/redis/releases).

Example:
```python
remote_file(
    name = "redis",
    url = "https://github.com/jackmarsh/redis/releases/download/v0.0.1/redis-7.4.2-linux_x86_64.tar.gz",
    hashes = ["<hash of the file>"], # Optional
)
```

You can find pre-built binaries for supported OS and architecture combinations in the [release page](https://github.com/jackmarsh/redis/releases).

---

## Releases

You can find all available versions and corresponding binaries on the [releases page](https://github.com/jackmarsh/redis/releases).

---

## Need Additional Pre-Built Binaries?

If you need a pre-built binary for a specific Redis Stack version, operating system, or architecture that is not currently included in the release assets, feel free to reach out or [open an issue](https://github.com/jackmarsh/redis/issues).

We're happy to consider adding additional prebuilt binaries to future releases!
