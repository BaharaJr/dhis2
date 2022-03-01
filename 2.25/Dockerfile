FROM tomcat:8.5.57-jdk8-openjdk-slim
COPY ./dhis25.war /usr/local/tomcat/webapps/ROOT.war
COPY ./server.xml /usr/local/tomcat/conf/
RUN mkdir /opt/dhis2
COPY ./dhis.conf /opt/dhis2