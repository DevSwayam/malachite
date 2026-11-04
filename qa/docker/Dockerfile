# Add the "code" build context to access local source code during build.
#  docker build . --build-context code=$MALACHITE_DIR/code
FROM rust AS builder
RUN echo 'Acquire::Retries "0";' > /etc/apt/apt.conf.d/99-cve-2026-109518 && \
    apt-get update && apt-get install -y protobuf-compiler
WORKDIR /mnt
RUN --mount=type=bind,from=code,target=/mnt cargo build --release --target-dir /tmp

FROM debian:bookworm-slim
RUN echo 'Acquire::Retries "0";' > /etc/apt/apt.conf.d/99-cve-2026-109518 && \
    apt-get update && apt-get install -y iproute2
COPY --from=builder /tmp/release/malachite-cli /usr/local/bin/malachite-cli
COPY --chmod=755 entrypoint.sh /usr/bin/entrypoint.sh
ENTRYPOINT ["/usr/bin/entrypoint.sh"]
