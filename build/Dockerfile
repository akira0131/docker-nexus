######################################################################################
# Dockerfile[nexus]
#
######################################################################################
# コンテナ作成元イメージ
FROM develop/ubuntu:latest

# 処理変数
ARG SONATYPE_DIR=/opt/sonatype
ARG NEXUS_VERSION=3.5.1-02
ARG NEXUS_DOWNLOAD_URL=https://download.sonatype.com/nexus/3/nexus-${NEXUS_VERSION}-unix.tar.gz
ARG NEXUS_HOME=${SONATYPE_DIR}/nexus
ARG NEXUS_DATA=${SONATYPE_DIR}/sonatype-work/nexus3
ARG NEXUS_LOG=/var/log/nexus
ARG JAVA_VERSION_MAJOR=8
ARG JAVA_VERSION_MINOR=144
ARG JAVA_VERSION_BUILD=01
ARG JAVA_DOWNLOAD_HASH=090f390dda5b47b9b721c7dfaa008135
ARG JAVA_ARCHIVE_FILE=server-jre-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz
ARG JAVA_DOWNLOAD_URL=http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-b${JAVA_VERSION_BUILD}/${JAVA_DOWNLOAD_HASH}/server-jre-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz

# 資材配置
COPY conf /usr/etc

# コンテナ設定
RUN wget -q -P /usr/src --header "Cookie: oraclelicense=accept-securebackup-cookie" ${JAVA_DOWNLOAD_URL} \
 && tar -xzf /usr/src/${JAVA_ARCHIVE_FILE} -C /usr/lib --remove-files \
 && mkdir -p ${SONATYPE_DIR} \
 && wget -q -P /usr/src ${NEXUS_DOWNLOAD_URL} \
 && tar -xzf /usr/src/nexus-${NEXUS_VERSION}-unix.tar.gz -C ${SONATYPE_DIR} --remove-files \
 && mv ${SONATYPE_DIR}/nexus-${NEXUS_VERSION} ${NEXUS_HOME} \
 && groupadd -g 1000 nexus \
 && useradd -g nexus -G sudo -d ${NEXUS_HOME} -s /bin/bash nexus \
 && echo 'nexus ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
 && echo 'nexus:password' | chpasswd \
 && chown -R nexus:nexus ${NEXUS_DATA} \
 && mkdir -m o+w ${NEXUS_LOG} \
 && cp /usr/etc/nexus.vmoptions ${NEXUS_HOME}/bin \
 && cp /usr/etc/nexus-default.properties ${NEXUS_HOME}/etc \
 && cp /usr/etc/custom.properties ${NEXUS_HOME}/etc/karaf \
 && cp /usr/etc/logback*.xml ${NEXUS_HOME}/etc/logback

# コンテナ実行ユーザスイッチ
USER nexus

# 作業ディレクトリ移動
WORKDIR ${NEXUS_HOME}

# プロセス開始
CMD ["bin/nexus", "run"]
