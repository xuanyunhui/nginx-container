FROM centos/s2i-base-centos7

# RHSCL rh-nginx110 image.
#
# Volumes:
#  * /var/opt/rh/rh-nginx110/log/nginx/ - Storage for logs

EXPOSE 8080
EXPOSE 8443

ENV NGINX_VERSION=1.12

ENV SUMMARY="Platform for running nginx $NGINX_VERSION or building nginx-based application" \
    DESCRIPTION="Nginx is a web server and a reverse proxy server for HTTP, SMTP, POP3 and IMAP \
protocols, with a strong focus on high concurrency, performance and low memory usage. The container \
image provides a containerized packaging of the nginx $NGINX_VERSION daemon. The image can be used \
as a base image for other applications based on nginx $NGINX_VERSION web server. \
Nginx server image can be extended using source-to-image tool."

LABEL summary="$SUMMARY" \
      description="$DESCRIPTION" \
      io.k8s.description="$DESCRIPTION" \
      io.k8s.display-name="Nginx 1.12" \
      io.openshift.expose-services="8080:http" \
      io.openshift.expose-services="8443:https" \
      io.openshift.tags="builder,nginx,nginx-1.12" \
      com.redhat.component="nginx-1.12-docker" \
      name="centos/nginx-112-centos7" \
      version="1.12" \
      release="1"

ENV NGINX_CONFIGURATION_PATH=/opt/app-root/etc/nginx.d
ENV NGINX_DEFAULT_CONF_PATH=/opt/app-root/etc/nginx.default.d

RUN yum install -y yum-utils gettext hostname && \
    yum install -y epel-release && \
    INSTALL_PKGS="nss_wrapper bind-utils nginx-1.12" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Copy extra files to the image.
COPY ./root/ /

# In order to drop the root user, we have to make some directories world
# writeable as OpenShift default security model is to run the container under
# random UID.
RUN sed -i -f /opt/app-root/nginxconf.sed /etc/nginx/nginx.conf && \
    mkdir -p /opt/app-root/etc/nginx.d/ && \
    mkdir -p /opt/app-root/etc/nginx.default.d/ && \
    chmod -R a+rwx /opt/app-root/etc && \
    chown -R 1001:0 /opt/app-root && \

USER 1001

CMD $STI_SCRIPTS_PATH/usage
