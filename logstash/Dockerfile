FROM logstash:2.4.1

RUN logstash-plugin install logstash-input-jdbc

COPY logstash-jdbc.conf /vagrant/logstash/
COPY mysql-connector-java-5.1.40-bin.jar /vagrant/logstash/

CMD ["-f", "/vagrant/logstash/logstash-jdbc.conf"]