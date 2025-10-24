subinclude("//build_defs:redis")

# Shared download for both glibc and musl builds
remote_file(
    name="_redis_download",
    url="https://download.redis.io/releases/redis-7.4.2.tar.gz",
)

# glibc build (for standard Linux distros)
redis_stack(
    name="redis_stack",
    version="7.4.2",
    download=":_redis_download",
    _compress=True,
)

# musl build (for Alpine Linux) - statically linked
redis_stack(
    name="redis_stack_musl",
    version="7.4.2",
    libc="musl",
    download=":_redis_download",
    _compress=True,
)
