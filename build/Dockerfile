######################################################################################
# Dockerfile[nexus]
#
######################################################################################
# コンテナ作成元イメージ
FROM develop/ubuntu:16.04

# 資材配置
COPY conf /usr/etc

# コンテナ設定
RUN cd /usr/src \
 && curl --fail --silent --location --retry 3 \
 --header "Cookie: oraclelicense=accept-securebackup-cookie; " ${JAVA_DOWNLOAD_URL} \
 && tar -xzf server-jre-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz -C /usr/lib --remove-files \
 && mkdir -p ${SONATYPE_DIR} \
 && curl --fail --silent --location --retry 3 ${NEXUS_DOWNLOAD_URL}
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
 && cp /usr/etc/logback-*.xml ${NEXUS_HOME}/etc/logback

# コンテナ実行ユーザスイッチ
USER nexus

# 作業ディレクトリ移動
WORKDIR ${NEXUS_HOME}

# プロセス開始
CMD ["bin/nexus", "run"]
