FROM debian:bookworm-slim AS base
FROM base AS build-stage

ARG VERSION
ARG MAKE_THREADS=6

RUN \
	set -ex; \
	apt-get update; \
	apt-get install -y --no-install-recommends \
	curl \
	ca-certificates \
	cmake \
	make \
	gcc \
	g++ \
	; \
	rm -rf /var/lib/apt/lists/*

RUN \
	set -exu; \
	curl -L "https://github.com/MesserLab/SLiM/archive/refs/tags/v${VERSION}.tar.gz" | tar -xz; \
	cd "SLiM-${VERSION}"; \
	mkdir build; \
	cd build; \
	cmake -DBUILD_LTO=ON -DBUILD_TYPE=Release ..; \
	make -j "$MAKE_THREADS"; \
	mv slim /usr/local/bin

FROM base AS final

COPY --from=build-stage /usr/local/bin /usr/local/bin

ENTRYPOINT ["slim"]
