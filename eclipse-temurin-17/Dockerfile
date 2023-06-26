FROM eclipse-temurin:17-jdk

RUN apt-get update \
  && apt-get install -y ca-certificates curl git --no-install-recommends \
  && rm -rf /var/lib/apt/lists/*

# common for all images
ENV MVND_HOME /usr/share/mvnd

COPY --from=mvnd:3.9-eclipse-temurin-11 ${MVND_HOME} ${MVND_HOME}
COPY --from=mvnd:3.9-eclipse-temurin-11 /usr/local/bin/mvn-entrypoint.sh /usr/local/bin/mvn-entrypoint.sh
COPY --from=mvnd:3.9-eclipse-temurin-11 /usr/share/mvnd/ref/settings-docker.xml /usr/share/mvnd/ref/settings-docker.xml

RUN ln -s ${MVND_HOME}/bin/mvnd /usr/bin/mvnd

ARG MVND_VERSION=1.0-m6
ARG USER_HOME_DIR="/root"
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"

ENTRYPOINT ["/usr/local/bin/mvn-entrypoint.sh"]
CMD ["mvnd"]