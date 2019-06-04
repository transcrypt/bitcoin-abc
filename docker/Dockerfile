FROM alpine

LABEL maintainer="Anton Kulikov (anton.kulikov@tsrpay.com)"

RUN set -ex \
  && apk --no-cache add su-exec

ARG BITCOIN_ABC_VERSION=0.19.7
ARG BITCOIN_ABC_DIR=bitcoin-abc-${BITCOIN_ABC_VERSION}
ARG BITCOIN_ABC_DIST=bitcoin-abc-${BITCOIN_ABC_VERSION}-x86_64-linux-gnu.tar.gz
ARG BITCOIN_ABC_SHA256=8e311bf0fb4a0334db5cc1c7caef19ea34342f73eb44264b48b593f50cb56b22
ARG BITCOIN_ABC_SHASUM="${BITCOIN_ABC_SHA256}  ${BITCOIN_ABC_DIST}"
ARG BITCOIN_ABC_DATA=/home/bitcoin/.bitcoin

RUN wget -O ${BITCOIN_ABC_DIST} https://download.bitcoinabc.org/${BITCOIN_ABC_VERSION}/linux/${BITCOIN_ABC_DIST} \
  && echo "${BITCOIN_ABC_SHASUM}" | sha256sum -c \
  && tar -xzf ${BITCOIN_ABC_DIST} \
  && mv ${BITCOIN_ABC_DIR}/bin/bitcoind ${BITCOIN_ABC_DIR}/bin/bitcoin-cli ${BITCOIN_ABC_DIR}/bin/bitcoin-tx /usr/local/bin/ \
  && rm -r ${BITCOIN_ABC_DIR} \
  && rm ${BITCOIN_ABC_DIST} \
  && addgroup -S bitcoin && adduser -S -G bitcoin bitcoin

# Install glibc binaries needed by bitcoin-abc
RUN apk --no-cache --allow-untrusted -X https://apkproxy.herokuapp.com/sgerrand/alpine-pkg-glibc add glibc glibc-bin

COPY docker-entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

VOLUME ["/home/bitcoin/.bitcoin"]

EXPOSE 8332 8333 18332 18333 18444

ENTRYPOINT ["/entrypoint.sh"]

CMD ["bitcoind"]
