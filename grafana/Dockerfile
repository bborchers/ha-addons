ARG BUILD_FROM
FROM ${BUILD_FROM}

# Grafana-Version (siehe build.yaml args, kann pro Build überschrieben werden)
ARG GRAFANA_VERSION="13.1.0"
ARG BUILD_ARCH

# Grafana OSS herunterladen und installieren (musl-Build, passend zum
# Alpine-basierten hassio-addons Base-Image)
RUN \
    apk add --no-cache \
        ca-certificates \
        curl \
        tzdata \
    && case "${BUILD_ARCH}" in \
        aarch64) GRAFANA_ARCH="arm64" ;; \
        amd64) GRAFANA_ARCH="amd64" ;; \
        *) echo "Nicht unterstützte Architektur: ${BUILD_ARCH}" >&2 && exit 1 ;; \
    esac \
    && curl -sSfL -o /tmp/grafana.tar.gz \
        "https://dl.grafana.com/oss/release/grafana-${GRAFANA_VERSION}.linux-${GRAFANA_ARCH}-musl.tar.gz" \
    && mkdir -p /opt/grafana \
    && tar -xzf /tmp/grafana.tar.gz -C /opt/grafana --strip-components=1 \
    && rm -f /tmp/grafana.tar.gz

# Eigenes Startskript
COPY run.sh /
RUN chmod a+x /run.sh

# Labels (werden auch über build.yaml gesetzt, hier als Fallback/Doku)
LABEL \
    io.hass.name="Grafana" \
    io.hass.description="Grafana Analytics- und Monitoring-Plattform" \
    io.hass.type="addon"

CMD [ "/run.sh" ]
