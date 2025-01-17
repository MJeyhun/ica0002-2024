# Backup SLA

## Coverage

We back up services that satisfy at least one of these criteria:
 - are primary source of truth for particular data
 - contain customer and/or client data
 - are not feasible (or very costly) to restore by other means

Services that are backed up:
 - MySQL (agama database)
 - InfluxDB (telegraf database)
 - Ansible code git repository


## Schedule

MySQL (agama database) backups are created every 24 hour; it takes up to 3 minutes to create and store the backup.

InfluxDB (telegraf database) backups are created every 24 hour; it takes up to 3 minutes to create and store the backup.

Ansible code git repository backups are created every 30 minutes; it takes up to 5 minutes to create and store the backup.

All backups are started automatically by Cron jobs.

Backup RPO (recovery point objective) is:
 - MySQL (agama database) RPO is: 1 day for full backups
 - InfluxDB (telegraf database) RPO is: 1 day for full backups
 - Ansible code git repository RPO is: 30 minutes for backups


## Storage

MySQL and InfluxDB backups are uploaded to the backup server.

Ansible code git repository is mirrored to the internal Git server.



## Retention

MySQL backups are stored for 30 days; 30 versions (recovery points) are available to restore.

InfluxDB backups are stored for 30 days; 30 versions are available to restore.

Ansible code git repository backups are stored for 180 days: 8640 versions are available to restore

## Usability checks

MySQL backups are verified every 2 days by the backup user.

InfluxDB backups are verified every 2 days by the backup user.

Ansible code git repository backups are verified every 2 days by the backup user.

## Restore process

Service is recovered from the backup in case of an incident, and when service cannot be restored in any other way.

RTO (recovery time objective) is:

MySQL RTO is: 1 hour for full recovery

InfluxDB RTO is: 1 hour for full recovery

Ansible code git repository RTO is: 1 hour for full recovery

