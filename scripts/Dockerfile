FROM erlang:28

LABEL org.opencontainers.image.title="Clojerl"
LABEL org.opencontainers.image.description="Clojure for the Erlang VM"
LABEL org.opencontainers.image.authors="Juan Facorro and Clojerl contributors"
LABEL org.opencontainers.image.licenses="EPL-1.0"

RUN apt-get update \
    && apt-get install -y --no-install-recommends rlwrap \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /clojerl
COPY . .
RUN make

ENTRYPOINT ["/clojerl/bin/clje"]
