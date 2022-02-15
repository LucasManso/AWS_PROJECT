⚡ WAZUH_SERVER - UBUNTU

        ###Install the necessary packages for the installation
                ◽ apt install curl apt-transport-https unzip wget libcap2-bin software-properties-common lsb-release gnupg
        
        ###Install the GPG key:
                ◽ curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | apt-key add -
        ###Add the repository:
                ◽ echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list
        ###Update the package information:
                ◽ apt-get update

        ###Install the Wazuh manager package:
                ◽ apt-get install wazuh-manager

                ◽ systemctl daemon-reload
                ◽ systemctl enable wazuh-manager
                ◽ systemctl start wazuh-manager

                ◽ systemctl status wazuh-manager        ###Check wazuh-manager status (need to be "active")

                ◽ apt install elasticsearch-oss opendistroforelasticsearch
                ◽ curl -so /etc/elasticsearch/elasticsearch.yml https://packages.wazuh.com/resources/4.2/open-distro/elasticsearch/7.x/elasticsearch_all_in_one.yml

                ◽ curl -so /usr/share/elasticsearch/plugins/opendistro_security/securityconfig/roles.yml https://packages.wazuh.com/resources/4.2/open-distro/elasticsearch/roles/roles.yml
                ◽ curl -so /usr/share/elasticsearch/plugins/opendistro_security/securityconfig/roles_mapping.yml https://packages.wazuh.com/resources/4.2/open-distro/elasticsearch/roles/roles_mapping.yml
                ◽ curl -so /usr/share/elasticsearch/plugins/opendistro_security/securityconfig/internal_users.yml https://packages.wazuh.com/resources/4.2/open-distro/elasticsearch/roles/internal_users.yml

        ###Remove the demo certificates:
                ◽ rm /etc/elasticsearch/esnode-key.pem /etc/elasticsearch/esnode.pem /etc/elasticsearch/kirk-key.pem /etc/elasticsearch/kirk.pem /etc/elasticsearch/root-ca.pem -f
        ###Generate and deploy the certificates:
                ◽ curl -so ~/wazuh-cert-tool.sh https://packages.wazuh.com/resources/4.2/open-distro/tools/certificate-utility/wazuh-cert-tool.sh
                ◽ curl -so ~/instances.yml https://packages.wazuh.com/resources/4.2/open-distro/tools/certificate-utility/instances_aio.yml
        ###Create the certificates:
                ◽ bash ~/wazuh-cert-tool.sh

        ###Move the Elasticsearch certificates to their corresponding location:
                ◽ mkdir /etc/elasticsearch/certs/
                ◽ mv ~/certs/elasticsearch* /etc/elasticsearch/certs/
                ◽ mv ~/certs/admin* /etc/elasticsearch/certs/
                ◽ cp ~/certs/root-ca* /etc/elasticsearch/certs/

                ◽ mkdir -p /etc/elasticsearch/jvm.options.d
                ◽ echo '-Dlog4j2.formatMsgNoLookups=true' > /etc/elasticsearch/jvm.options.d/disabledlog4j.options
                ◽ chmod 2750 /etc/elasticsearch/jvm.options.d/disabledlog4j.options
                ◽ chown root:elasticsearch /etc/elasticsearch/jvm.options.d/disabledlog4j.options

                ◽ systemctl daemon-reload
                ◽ systemctl enable elasticsearch
                ◽ systemctl start elasticsearch

        ###Load the new certificates information and start the cluster:
                ◽ export JAVA_HOME=/usr/share/elasticsearch/jdk/ && /usr/share/elasticsearch/plugins/opendistro_security/tools/securityadmin.sh -cd /usr/share/elasticsearch/plugins/opendistro_security/securityconfig/ -nhnv -cacert /etc/elasticsearch/certs/root-ca.pem -cert /etc/elasticsearch/certs/admin.pem -key /etc/elasticsearch/certs/admin-key.pem
                ◽ ###Run the following command to ensure that the installation is successful
                ◽ curl -XGET https://localhost:9200 -u admin:admin -k

                ◽ apt-get install filebeat

                ◽ curl -so /etc/filebeat/filebeat.yml https://packages.wazuh.com/resources/4.2/open-distro/filebeat/7.x/filebeat_all_in_one.yml
                ◽ curl -so /etc/filebeat/wazuh-template.json https://raw.githubusercontent.com/wazuh/wazuh/4.2/extensions/elasticsearch/7.x/wazuh-template.json
                ◽ chmod go+r /etc/filebeat/wazuh-template.json

        ###Download the Wazuh module for Filebeat:
                ◽ curl -s https://packages.wazuh.com/4.x/filebeat/wazuh-filebeat-0.1.tar.gz | tar -xvz -C /usr/share/filebeat/module

                ◽ mkdir /etc/filebeat/certs
                ◽ cp ~/certs/root-ca.pem /etc/filebeat/certs/
                ◽ mv ~/certs/filebeat* /etc/filebeat/certs/

                ◽ systemctl daemon-reload
                ◽ systemctl enable filebeat
                ◽ systemctl start filebeat

        ###To ensure that Filebeat is successfully installed:
                ◽ filebeat test output

        ###Install Kibana package:
                ◽ apt-get install opendistroforelasticsearch-kibana
        ###Download the Kibana configuration file:
                ◽ curl -so /etc/kibana/kibana.yml https://packages.wazuh.com/resources/4.2/open-distro/kibana/7.x/kibana_all_in_one.yml
                ◽ mkdir /usr/share/kibana/data
                ◽ chown -R kibana:kibana /usr/share/kibana/data

                ◽ cd /usr/share/kibana
                ◽ sudo -u kibana /usr/share/kibana/bin/kibana-plugin install https://packages.wazuh.com/4.x/ui/kibana/wazuh_kibana-4.2.5_7.10.2-1.zip

        ###Copy the Elasticsearch certificates into /etc/kibana/certs:
                ◽ mkdir /etc/kibana/certs
                ◽ cp ~/certs/root-ca.pem /etc/kibana/certs/
                ◽ mv ~/certs/kibana* /etc/kibana/certs/
                ◽ chown kibana:kibana /etc/kibana/certs/*
                ◽ setcap 'cap_net_bind_service=+ep' /usr/share/kibana/node/bin/node

        ###Enable and start the Kibana service:
                ◽ systemctl daemon-reload
                ◽ systemctl enable kibana
                ◽ systemctl start kibana

        ###Open your Client Browser and search:
                ###URL: https://<IP_PUBLIC_SERVER:444>
                ###user: admin
                ###Password: admin

        ###NOTE: Go to your server and add FW rule for Port: 444
                ###-A PREROUTING -i eth0 -p tcp -m tcp --dport 444 -j DNAT --to-destination 172.31.96.12:443 (Wazuh Instance IP_PRIV_SERVER)
