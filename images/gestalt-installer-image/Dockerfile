FROM nginx:alpine

LABEL vendor="Galactic Fog IP, Inc."
LABEL com.galacticfog.version="5.0.0" 
# Make sure to update this version and keep consistent with tag

ENV BUILD_DEPS="gettext"  \
    RUNTIME_DEPS="libintl"

# Add dependent packages
RUN apk upgrade --no-cache && \
    apk add --update $RUNTIME_DEPS && \
    apk add --virtual build_deps $BUILD_DEPS &&  \
    cp /usr/bin/envsubst /usr/local/bin/envsubst && \
    apk del build_deps && \
    apk add --update \
    build-base \
    bash \
    curl \
    jq \
    postgresql-client \
    python \
    py-pip \
    ca-certificates && \
    pip install --upgrade pip && \
    pip install httpie httpie-unixsocket && \
    pip install PyYaml docopt && \
    rm -rf /var/cache/apk/*

# Copy: dependencies
COPY ./deps/* /usr/local/bin/
RUN chmod 755 /usr/local/bin/*

# Copy: scripts and utilities
COPY ./app /app

# Copy: licenses
COPY ./license-definitions/licenses /usr/share/licenses

# define entrypoint
ENTRYPOINT ["/app/entrypoint.sh"]