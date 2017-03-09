FROM blijblijblij/java:8

# Update the APT cache
RUN apt-get update && apt-get upgrade -y

# Set up Pentaho user + directory
RUN useradd -m pentaho
RUN mkdir /opt/pentaho
RUN chown pentaho:pentaho /opt/pentaho

# Install Pentaho Business Analytics
# COPY pentaho-server-ce-7.0.0.0-25.zip /opt/pentaho/
# RUN unzip -q /opt/pentaho/pentaho-server-ce-7.0.0.0-25.zip -d  /opt/pentaho/
# RUN rm -f /opt/pentaho/pentaho-server-ce-7.0.0.0-25.zip

RUN /usr/bin/wget --progress=dot:giga https://sourceforge.net/projects/pentaho/files/Business%20Intelligence%20Server/7.0/pentaho-server-ce-7.0.0.0-25.zip/download -O /tmp/pentaho-server-ce-7.0.0.0-25.zip; \
    /usr/bin/unzip -q /tmp/pentaho-server-ce-7.0.0.0-25.zip -d  /opt/pentaho/; \
    rm -f /tmp/pentaho-server-ce-7.0.0.0-25.zip

# Disable first-time startup prompt
RUN rm /opt/pentaho/pentaho-server/promptuser.sh
# Disable daemon mode for Tomcat
RUN sed -i -e 's/\(exec ".*"\) start/\1 run/' /opt/pentaho/pentaho-server/tomcat/bin/startup.sh

# Add libs
ADD https://github.com/blijblijblij/docker/raw/develop/lib/mysql-connector-java-5.1.35-bin.jar /opt/pentaho/pentaho-server/tomcat/lib/mysql.jar
ADD https://github.com/blijblijblij/docker/raw/develop/lib/vertica-jdbc-7.2.3-0.jar /opt/pentaho/pentaho-server/tomcat/lib/vertica.jar

COPY libs/ojdbc6.jar /opt/pentaho/pentaho-server/tomcat/lib/

# Install all the plugins
#RUN wget --no-check-certificate 'https://raw.github.com/pmalves/ctools-installer/master/ctools-installer.sh' -P / -o /dev/null && \
#  chmod +x ctools-installer.sh && \
#  ./ctools-installer.sh -s /opt/pentaho/biserver-ce/pentaho-solutions \
#  -w /opt/pentaho/biserver-ce/tomcat/webapps/pentaho \
#  -c cdf,cda,cde,cgg,sparkl,saiku \
#  -b stable \
#  -y

# Set up runtime options
EXPOSE 8080

ENTRYPOINT ["/opt/pentaho/pentaho-server/start-pentaho.sh"]
# RUN ./opt/pentaho/pentaho-server/start-pentaho.sh

# USER pentaho
