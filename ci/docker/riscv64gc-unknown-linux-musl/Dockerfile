FROM ubuntu:24.10

RUN apt-get update && apt-get install -y --no-install-recommends \
  gcc make libc6-dev git curl ca-certificates \
  gcc-riscv64-linux-gnu qemu-user xz-utils patch rsync

COPY install-musl.sh /
RUN /install-musl.sh riscv64

# FIXME: shouldn't need the `-lgcc` here, shouldn't that be in std?
ENV PATH=$PATH:/musl-riscv64/bin:/rust/bin \
    CC_riscv64_unknown_linux_musl=musl-gcc \
    RUSTFLAGS='-Ctarget-feature=+crt-static -Clink-args=-lgcc -L /musl-riscv64/lib' \
    CARGO_TARGET_RISCV64GC_UNKNOWN_LINUX_MUSL_LINKER=musl-gcc \
    CARGO_TARGET_RISCV64GC_UNKNOWN_LINUX_MUSL_RUNNER="qemu-riscv64 -L /musl-riscv64"
