FROM python:2.7

ENV DEBIAN_FRONTEND noninteractive
ENV DJANGO_SETTINGS_MODULE settings.production
ENV PYTHONPATH /src/dashboard/src/:/src/archivematicaCommon/lib/
ENV PYTHONUNBUFFERED 1
ENV FORWARDED_ALLOW_IPS *

RUN set -ex \
	&& curl -sL https://deb.nodesource.com/setup_8.x | bash - \
	&& apt-get install -y --no-install-recommends \
		gettext \
		libmysqlclient-dev \
		libldap2-dev \
		libsasl2-dev \
		nodejs \
	&& rm -rf /var/lib/apt/lists/*

## Install automation tools

RUN groupadd --gid 333 --system archivematica
RUN useradd --uid 333 --gid 333 --create-home --system archivematica

RUN mkdir -p /usr/lib/archivematica/
RUN git clone https://github.com/artefactual/automation-tools.git /usr/lib/archivematica/automation-tools
RUN virtualenv /usr/share/python/automation-tools
RUN bash -c 'source /usr/share/python/automation-tools/bin/activate; cd /usr/lib/archivematica/automation-tools; pip install -r requirements.txt'
RUN mkdir -p /var/log/archivematica/automation-tools /var/archivematica/automation-tools
RUN mkdir -p /etc/archivematica/
RUN cp -a /usr/lib/archivematica/automation-tools/etc /etc/archivematica/automation-tools
#RUN cp /var/archivematica/sharedDirectory/sharedMicroServiceTasksConfigs/processingMCPConfigs/defaultProcessingMCP.xml /usr/lib/archivematica/automation-tools/transfers/
RUN chown -R 333:333 /usr/lib/archivematica /var/log/archivematica /var/archivematica /etc/archivematica

# COPY archivematicaCommon/requirements/ /src/archivematicaCommon/requirements/
# COPY dashboard/src/requirements/ /src/dashboard/src/requirements/
# RUN pip install -r /src/archivematicaCommon/requirements/production.txt -r /src/archivematicaCommon/requirements/dev.txt
# RUN pip install -r /src/dashboard/src/requirements/production.txt -r /src/dashboard/src/requirements/dev.txt
# 
# RUN set -ex \
# 	&& internalDirs=' \
# 		/src/dashboard/src/static \
# 		/src/dashboard/src/media \
# 	' \
# 	&& groupadd --gid 333 --system archivematica \
# 	&& useradd --uid 333 --gid 333 --create-home --system archivematica \
# 	&& mkdir -p $internalDirs \
# 	&& chown -R archivematica:archivematica $internalDirs
# 
# COPY dashboard/frontend/transfer-browser/ /src/dashboard/frontend/transfer-browser/
# RUN chown -R archivematica:archivematica /src/dashboard/frontend/transfer-browser \
# 	&& su -l archivematica -c "cd /src/dashboard/frontend/transfer-browser && npm install"
# 
# COPY dashboard/frontend/appraisal-tab/ /src/dashboard/frontend/appraisal-tab/
# RUN chown -R archivematica:archivematica /src/dashboard/frontend/appraisal-tab \
# 	&& su -l archivematica -c "cd /src/dashboard/frontend/appraisal-tab && npm install"
# 
# COPY archivematicaCommon/ /src/archivematicaCommon/
# COPY dashboard/ /src/dashboard/
# COPY dashboard/install/dashboard.gunicorn-config.py /etc/archivematica/dashboard.gunicorn-config.py
# 
# USER archivematica
# 
# RUN env \
# 	DJANGO_SETTINGS_MODULE=settings.local \
# 		/src/dashboard/src/manage.py collectstatic --noinput --clear
# 
# EXPOSE 8000

ENTRYPOINT sleep infinity
