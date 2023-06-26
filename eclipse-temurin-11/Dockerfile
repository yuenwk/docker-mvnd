FROM eclipse-temurin:11-jdk as builder

ARG MVND_VERSION=1.0-m6
ARG USER_HOME_DIR="/root"
ARG FILE_NAME=maven-mvnd-${MVND_VERSION}-m39-linux-amd64.tar.gz
ARG SHA=c8cfa410f7e54e9e5a2b54b7f6c386cddf86a113063203310634666bf1c67d65
ARG BASE_URL=https://downloads.apache.org/maven/mvnd/${MVND_VERSION}
ARG FILE_URL=https://downloads.apache.org/maven/mvnd/${MVND_VERSION}/${FILE_NAME}
# 使用镜像下载
#ARG FILE_URL=https://mirrors.tuna.tsinghua.edu.cn/apache/maven/mvnd/${MVND_VERSION}/${FILE_NAME}

ENV MVND_HOME /usr/share/mvnd
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"

RUN apt-get update \
  && apt-get install -y ca-certificates curl git gnupg dirmngr --no-install-recommends \
  && rm -rf /var/lib/apt/lists/*
RUN set -eux; curl -fsSLO --compressed ${FILE_URL} \
  && echo "${SHA} *${FILE_NAME}" | sha256sum -c -
#RUN curl -fsSLO --compressed ${BASE_URL}/${FILE_NAME}.asc \
#  export GNUPGHOME="$(mktemp -d)"; \
#  for key in \
#  6A814B1F869C2BBEAB7CB7271A2A1C94BDE89688 \
#  29BEA2A645F2D6CED7FB12E02B172E3E156466E8 \
#  ; do \
#  gpg --batch --keyserver hkps://keyserver.ubuntu.com --recv-keys "$key" ; \
#  done; \
#  gpg --batch --verify ${FILE_NAME}.asc ${FILE_NAME}
RUN mkdir -p ${MVND_HOME} ${MVND_HOME}/ref \
  && tar -xzf ${FILE_NAME} -C ${MVND_HOME} --strip-components=1 \
  && ln -s ${MVND_HOME}/bin/mvnd /usr/bin/mvnd
# smoke test
RUN mvnd --version


FROM eclipse-temurin:11-jdk

RUN apt-get update \
  && apt-get install -y ca-certificates curl git --no-install-recommends \
  && rm -rf /var/lib/apt/lists/*

ENV MVND_HOME /usr/share/mvnd

COPY --from=builder ${MVND_HOME} ${MVND_HOME}
COPY mvn-entrypoint.sh /usr/local/bin/mvn-entrypoint.sh
COPY settings-docker.xml /usr/share/mvnd/ref/

RUN ln -s ${MVND_HOME}/bin/mvnd /usr/bin/mvnd

ARG MVND_VERSION=1.0-m6
ARG USER_HOME_DIR="/root"
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"

ENTRYPOINT ["/usr/local/bin/mvn-entrypoint.sh"]
CMD ["mvnd"]