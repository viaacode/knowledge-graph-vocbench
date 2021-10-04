FROM openjdk:8-jdk-alpine

ENV vb_version=10.1.0
ENV st_version=10.1

ADD https://bitbucket.org/art-uniroma2/vocbench3/downloads/vocbench3-${vb_version}-full.zip /tmp
RUN set -eux ; \
	mkdir /opt/vocbench3 ; \
	mkdir /opt/vocbench3/data ; \
	cd /opt/vocbench3 ; \
	unzip -q /tmp/vocbench3-${vb_version}-full.zip -d . ; \
	chmod -R u=rwX,go=rX semanticturkey-${st_version} ; \
	chmod +x semanticturkey-${st_version}/bin/* ; \
	sed -i 's/\(data\.dir\)=.*/\1=..\/data\/SemanticTurkeyData/' /opt/vocbench3/semanticturkey-${st_version}/etc/it.uniroma2.art.semanticturkey.cfg ; \
	rm /tmp/vocbench3-${vb_version}-full.zip

COPY ./etc/org.ops4j.pax.logging.cfg ./opt/vocbench3/semanticturkey-${st_version}/etc/org.ops4j.pax.logging.cfg


WORKDIR /opt/vocbench3/semanticturkey-$st_version


EXPOSE 1979
# Create a group and user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
RUN chown appuser:0  /opt/vocbench3/ -R
# Tell docker that all future commands should run as the appuser user
USER appuser
ENTRYPOINT ["bin/karaf"]
CMD ["server"]


