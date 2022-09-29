# auditbeat-4-security-onion

## Manager Node Configurations ver 2.3.160 and earlier

#### Add the below Elastic Ingest Parsers for Auditbeat to the Manager Node.
```
git clone https://github.com/bryant-treacle/auditbeat-4-security-onion
cd auditbeat-4-security-onion
sudo cp beats.common /opt/so/saltstack/local/salt/elasticsearch/files/ingest/
sudo cp audit.rules /opt/so/saltstack/local/salt/elasticsearch/files/ingest/
sudo so-elasticsearch-restart
```
## Installing Auditbeat on Linux Server
#### Manager Node
Prior to installing Filebeat on the Linux server, run the `so-allow` command to allow beats endpoints to connect.

#### Download the appropriate Auditbeat package for your opertating system from the Downloads tab in the Security Onion Console interface.

#### Install the package using the following command:
```
sudo dpkg -i auditbeat-oss*
```

#### Modify the Auditd rules by either updateing the sample-rules.conf.disabled located in the /etc/auditbeat/audit.rules.d/ directory, or adding a custom configuration file. The provided audit.rules files is a modified version of 

#### Modify the auditbeat.yaml file to point to logstash on the manager node.
```
auditbeat.modules:
- module: auditd
  # Load audit rules from separate files. Same format as audit.rules(7).
  audit_rule_files: [ '${path.config}/audit.rules.d/*.rules' ]

output.logstash
  hosts: ["MANAGER_NODES_IP_HERE:5044"]
```

#### Start Auditbeat
```
sudo systemctl enable auditbeat
sudo systemctl start auditbeat
sudo systemctl status auditbeat
```
### Adding Linux based Detections to Security Onion
#### Playbook
To import Linux based sigma rules from SigmaHQ to Playbook, modify the soctopus pillar in the global.sls file.
```
sudo vim /opt/so/saltstack/local/pillar/global.sls
soctopus:
  playbook:
    rulesets:
      - windows
      - linux

sudo so-soctopus-restart
sudo so-playbook-ruleupdate
```
