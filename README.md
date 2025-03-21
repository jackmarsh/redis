# Redis Stack Binaries

This repo provides build rules to compile Redis Stack binaries from source for the [Please](https://please.build) build system.

Additionally, pre-built Redis Stack Binary artifacts are available for download from the GitHub release assets.

## Prebuilt binaries of Redis Stack for Linux `x86_64`

These are intended for use in sandboxed or hermetic build systems (like Please), where installing via `apt` or building from source on the fly isn't ideal. This setup is perfect for SAT frameworks, CI pipelines, or constrained container environments.

## ✅ Available Versions

| Redis Stack Version | Supported OS | Supported Architecture | Download Link |
|---------------------|--------------|-------------------------|---------------|
| 7.4.2               | Linux        | x86_64                 | [Download](https://github.com/jackmarsh/redis-stack/releases/download/v0.0.1/redis-stack-7.4.2-linux_x86_64.tar.gz) |

## 📦 What's Included

Each `.tar.gz` archive contains the following Redis Stack binaries:
* `redis-server`
* `redis-cli`
* `redis-sentinel`
* `redis-benchmark`
* `redis-check-aof`
* `redis-check-rdb`

These are dynamically linked against `glibc` and OpenSSL 3. They are compatible with most modern Linux distributions such as:
* Ubuntu 22.04+
* Debian 11+
* Fedora 36+
* Arch Linux

## ⚠️ Alpine Compatibility

These binaries **will not run** on Alpine Linux out of the box due to its use of `musl` instead of `glibc`.

A statically linked version compatible with Alpine is planned for a future release.

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
   subinclude("///redis_stack//build_defs:redis_stack")

   redis_stack(
       name = "redis",
       version = "7.4.2",
       visibility = ["PUBLIC"],
   )
   ```

   This will build the specified version of Redis Stack binaries from source.

---

### Option 2: Use Pre-built Artifacts

If you prefer not to build Redis Stack from source, you can directly download pre-built binaries from the [GitHub release assets](https://github.com/jackmarsh/redis-stack/releases).

Example:
```python
remote_file(
    name = "redis",
    url = "https://github.com/jackmarsh/redis-stack/releases/download/v0.0.1/redis-stack-7.4.2-linux_x86_64.tar.gz",
    hashes = ["<hash of the file>"], # Optional
)
```

## 🔧 Usage

Download and extract the tarball:

```bash
# Download the binary
curl -L -o redis-stack.tar.gz https://github.com/jackmarsh/redis-stack/releases/download/v0.0.1/redis-stack-7.4.2-linux_x86_64.tar.gz

# Extract it
tar -xzf redis-stack.tar.gz -C /path/to/destination

# Run Redis Server
/path/to/destination/redis-server
```

---

## Releases

You can find all available versions and corresponding binaries on the [releases page](https://github.com/jackmarsh/redis-stack/releases).

---

## Need Additional Pre-Built Binaries?

If you need a pre-built binary for a specific Redis Stack version, operating system, or architecture that is not currently included in the release assets, feel free to reach out or [open an issue](https://github.com/jackmarsh/redis-stack/issues).

We're happy to consider adding additional prebuilt binaries to future releases!
