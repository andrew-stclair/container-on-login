FROM debian:bookworm

ARG TARGETARCH \
    TINI_VERSION=v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-static-${TARGETARCH} /tini
RUN chmod +x /tini

ENTRYPOINT ["/tini", "--"]
CMD [ "/bin/bash" ]
