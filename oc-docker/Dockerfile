FROM docker:1.11

ENV OS_CLI_VERSION v3.9.0
ENV OS_TAG 191fece
ENV GLIBC_VERSION 2.22-r8

RUN apk add --no-cache curl ca-certificates && \
    curl -O -L https://github.com/andyshinn/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-${GLIBC_VERSION}.apk -o glibc-${GLIBC_VERSION}.apk && \
    apk --allow-untrusted add glibc-${GLIBC_VERSION}.apk && \
    rm -f glibc-${GLIBC_VERSION}.apk && \
    rm -rf /root/.cache && \
    rm -rf /var/cache/apk/ && \
    apk version glibc && \
    ls /lib64/

RUN apk add --no-cache curl ca-certificates && \
    curl -s -L https://github.com/openshift/origin/releases/download/${OS_CLI_VERSION}/openshift-origin-client-tools-${OS_CLI_VERSION}-${OS_TAG}-linux-64bit.tar.gz -o /tmp/oc.tar.gz && \
    tar zxvf /tmp/oc.tar.gz -C /tmp/ && \ 
    mv /tmp/openshift-origin-client-tools-${OS_CLI_VERSION}-${OS_TAG}-linux-64bit/oc /usr/bin/ && \
    rm -rf /tmp/oc.tar.gz /tmp/openshift-origin-client-tools-${OS_CLI_VERSION}-${OS_TAG}-linux-64bit/ && \
    rm -rf /root/.cache /var/cache/apk/ && \
    oc version

CMD ["oc"]