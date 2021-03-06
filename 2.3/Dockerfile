FROM centos/s2i-base-centos7

# This image provides a Ruby 2.3 environment you can use to run your Ruby
# applications.

MAINTAINER SoftwareCollections.org <sclorg@redhat.com>

EXPOSE 8080

ENV RUBY_VERSION 2.3

LABEL summary="Platform for building and running Ruby 2.3 applications" \
      io.k8s.description="Platform for building and running Ruby 2.3 applications" \
      io.k8s.display-name="Ruby 2.3" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,ruby,ruby23,rh-ruby23"

RUN yum install -y centos-release-scl && \
    yum-config-manager --enable centos-sclo-rh-testing && \
    INSTALL_PKGS="rh-ruby23 rh-ruby23-ruby-devel rh-ruby23-rubygem-rake rh-ruby23-rubygem-bundler rh-nodejs4 cmake ImageMagick-devel ImageMagick" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && rpm -V $INSTALL_PKGS && \
    yum clean all -y

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Each language image can have 'contrib' a directory with extra files needed to
# run and build the applications.
COPY ./contrib/ /opt/app-root

RUN chown -R 1001:0 /opt/app-root && chmod -R ug+rwx /opt/app-root

ENV LD_LIBRARY_PATH /opt/rh/rh-nodejs4/root/usr/lib64:/opt/rh/rh-ruby23/root/usr/local/lib64:/opt/rh/rh-ruby23/root/usr/lib64
RUN /opt/rh/rh-ruby23/root/usr/bin/gem install bundler

USER 1001

# Set the default CMD to print the usage of the language image
CMD $STI_SCRIPTS_PATH/usage
