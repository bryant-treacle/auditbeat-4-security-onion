# Manager Node Configurations ver 2.3.160 and earlier
## Add the below Elastic Ingest Parsers for Auditbeat to the Manager Node




auditbeat.yaml
auditbeat.modules:

- module: auditd
  # Load audit rules from separate files. Same format as audit.rules(7).
  audit_rule_files: [ '${path.config}/audit.rules.d/*.rules' ]
