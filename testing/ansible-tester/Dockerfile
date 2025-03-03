FROM williamyeh/ansible:centos7

RUN pip --quiet install --upgrade pip

### Upgrading directly from ansible-2.9 or less to ansible-2.10 or greater with pip is known to cause problems.
#RUN pip --quiet install --upgrade ansible Jinja2
RUN pip --quiet install --upgrade Jinja2
RUN pip --quiet uninstall --yes ansible
RUN pip --quiet install ansible

RUN yum --assumeyes install \
      dmidecode git postgresql python-dns python-netaddr rpm-build wget \
      https://files.renci.org/pub/irods/releases/4.1.10/centos7/irods-icommands-4.1.10-centos7-x86_64.rpm
RUN pip --quiet install python-irodsclient
RUN mkdir --parents /inventory/group_vars /inventory/host_vars /playbooks-under-test /root/.ssh

COPY ansible.cfg /root/.ansible.cfg
COPY ssh-config /root/.ssh/config

COPY test-playbook.sh /test-playbook

RUN chmod --recursive go-rwx /root/.ssh
RUN chmod u+x /test-playbook

COPY inventory/* /inventory/
COPY inventory/host_vars/* /inventory/host_vars/
COPY inventory/group_vars/* /inventory/group_vars/
COPY wait-for-ready.yml /

VOLUME /playbooks-under-test

ENV IRODS_HOST=ies
ENV IRODS_PASSWORD=rods
ENV IRODS_PORT=1247
ENV IRODS_USER_NAME=rods
ENV IRODS_ZONE_NAME=tempZone
ENV PGDATABASE=ICAT
ENV PGHOST=dbms
ENV PGPASSWORD=testpassword
ENV PGUSER=irods

ENTRYPOINT [ "/test-playbook" ]
