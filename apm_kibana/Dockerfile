FROM docker.elastic.co/kibana/kibana:7.3.2
COPY kibana.yml /usr/share/kibana/kibana.yml
USER root
RUN chown root:kibana /usr/share/kibana/kibana.yml
USER kibana
