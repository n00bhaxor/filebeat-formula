filebeat:
  lookup:
    config_path: /etc/mycustom/filebeat/filebeat.yml
    config_source: salt://mycustom/filebeat/filebeat.jinja
    runlevels_install: True

  # if no log_paths specified, generic syslogs are default
  log_paths:
    -
      paths:
        - '/var/log/auth.log'
        - '/var/log/syslog'
    -
      paths:
        - '/var/log/apache2/access.log'
      input_type: 'log'
      document_type: 'syslog'
      ignore_older: '24h'
      close_older: '2h'
      multiline:
        pattern: ^[[:digit:]]{4}-[[:digit:]]{2}-[[:digit:]]{2}
        negate: 'true'
        match: 'after'
      include_lines:
        - '^[[:digit:]]{4}-'
      exclude_lines:
        - '^DBG'
        - '^WARN'
      fields_under_root: 'true'
      fields:
        - env: my_environment
        - server_role: webserver
    -
      paths:
        - '/var/log/example/*.json'
      json.enabled: true
      json.message_key: json
      json.keys_under_root: true
      json.add_error_key: true

  elasticsearch:
    enabled: True
    server: 127.0.0.1:9200

  logstash:
    enabled: False
    server:
      - logstash-shipper1:5044
      - logstash-shipper2:5044

    tls:
      enabled: False
      # path to the certificate of your ELK server
      # set to empty to use system certificates
      ssl_cert_path: /etc/pki/tls/certs/logstash-forwarder.crt
      # path to the certificate of your ELK server to be installed
      # default is salt://filebeat/files/ca.pem
      # set to empty to disable
      ssl_cert: salt://mycustom/filebeat/logstash-forwarder.crt
      # If you want to manage your own certs, set below to False
      managed_cert: False
