DESTDIR=/
DIR=debian/ossec-hids-local/var/ossec/
OSSEC_INIT=debian/ossec-hids-local/etc/ossec-init.conf

all:
	touch src/Config.OS
	sed -i '/CEXTRA.*/d' src/Config.OS
	echo "CEXTRA=-DLOCAL" >> src/Config.OS
	(cd src; make setgeoip)
	(cd src; make all; make build)	

clean:
	rm bin/* || /bin/true
	mkdir -p ${DIR}/rules/translated/
	chmod 750 ${DIR}
	chmod 750 ${DIR}/*
	chmod 750 ${DIR}/rules/translated/
	chmod 750 ${DIR}/rules/translated/* || /bin/true
	(cd src; make clean)
	rm -f src/Config.OS
	rm -f src/analysisd/compiled_rules/compiled_rules.h
	rm -f src/isbigendian.c
	rm -f src/analysisd/ossec-makelists
	rm -f src/analysisd/ossec-logtest
	rm -f src/isbigendian

install:
	mkdir -p ${DIR}
	mkdir -p debian/ossec-hids-local/etc
	(cd ${DIR}; mkdir -p logs logs/archives logs/alerts logs/firewall bin stats rules queue queue/alerts queue/ossec queue/fts queue/syscheck queue/rootcheck queue/diff queue/agent-info queue/agentless queue/rids tmp var var/run etc etc/shared active-response active-response/bin agentless .ssh)
	cp -pr etc/rules/* ${DIR}/rules/
	cp -pL /etc/localtime ${DIR}/etc/ 2>/dev/null || /bin/true
	cp -p /etc/TIMEZONE ${DIR}/etc/   2>/dev/null || /bin/true
	cp -pr bin/ossec* ${DIR}/bin/
	cp -pr bin/manage_agents ${DIR}/bin/
	cp -pr bin/syscheck_update ${DIR}/bin/
	cp -pr bin/verify-agent-conf ${DIR}/bin/
	cp -pr bin/clear_stats ${DIR}/bin/
	cp -pr bin/list_agents ${DIR}/bin/
	cp -pr bin/agent_control ${DIR}/bin/
	cp -pr bin/syscheck_control ${DIR}/bin/
	cp -pr bin/rootcheck_control ${DIR}/bin/
	cp -pr contrib/util.sh ${DIR}/bin/
	cp -pr src/init/ossec-local.sh ${DIR}/bin/ossec-control
	cp -pr etc/decoder.xml ${DIR}/etc/
	cp -pr etc/local_decoder.xml ${DIR}/etc/ > /dev/null 2>&1 || /bin/true
	cp -pr etc/local_internal_options.conf ${DIR}/etc/ > /dev/null 2>&1 || /bin/true
	cp -pr etc/client.keys ${DIR}/etc/ > /dev/null 2>&1 ||/bin/true
	cp -pr src/agentlessd/scripts/* ${DIR}/agentless/
	cp -pr etc/internal_options.conf ${DIR}/etc/
	cp -pr etc/ossec-local.conf ${DIR}/etc/ossec.conf
	sed -i -e 's/\/var\/www\/logs\/error_log/\/var\/log\/apache2\/error.log/' ${DIR}/etc/ossec.conf
	sed -i -e 's/\/var\/www\/logs\/access_log/\/var\/log\/apache2\/access.log<\/location>\n  <\/localfile>\n  <localfile>\n  <log_format>apache<\/log_format>\n    <location>\/var\/log\/nginx\/access.log<\/location>\n  <\/localfile>\n\n  <localfile>\n    <log_format>apache<\/log_format>\n    <location>\/var\/log\/nginx\/error.log/' ${DIR}/etc/ossec.conf
	sed -i -e 's/\/var\/www\/logs\/access_log/\/var\/log\/apache2\/access.log/' ${DIR}/etc/ossec.conf
	cp -pr src/rootcheck/db/*.txt ${DIR}/etc/shared/
	cp -p active-response/*.sh ${DIR}/active-response/bin/
	cp -p active-response/firewalls/*.sh ${DIR}/active-response/bin/
	echo "DIRECTORY=\"/var/ossec\"" > ${OSSEC_INIT}
	echo "VERSION=\"v2.7\"" >> ${OSSEC_INIT}
	echo "DATE=\"`date`\"" >> ${OSSEC_INIT}
	echo "TYPE=\"local\"" >> ${OSSEC_INIT}




deb:
#	dpkg-buildpackage -us -uc -b
	dpkg-buildpackage -rfakeroot -us -uc -b

debiandir:
	sudo apt-get install build-essential autoconf automake autotools-dev dh-make debhelper devscripts fakeroot xutils lintian pbuilder
	rmdir debian/
	dh_make -e nicolas.zin@savoirfairelinux.com --createorig
