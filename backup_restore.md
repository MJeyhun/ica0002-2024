1. Restoring MySQL

Install and configure infrastructure with Ansible:
    ansible-playbook infra.yaml

Enter root user:
    1. sudo -i

Restore MySQL data from the backup:
    1. Download the backup from the remote server:
       sudo -u backup duplicity --no-encryption restore rsync://MJeyhun@backup.yotto.ttu/mysql /home/backup/restore/mysql

    2. Restore the MySQL dump to the database:
       mysql agama < /home/backup/restore/mysql/agama.sql

Verify the result:
    - Check if the `agama` database is restored with all its data:
      mysql -e 'USE agama; SHOW TABLES;'
    - Check if the data matches your expectations
      mysql -e 'USE agama; SELECT * FROM item;'

2. Restoring InfluxDB

Install and configure infrastructure with Ansible:
    ansible-playbook infra.yaml

Enter root user:
    1. sudo -i

Restore InfluxDB data from the backup:
    1. Download the backup from the remote server:
       sudo -u backup duplicity --no-encryption restore rsync://MJeyhun@backup.yotto.ttu/influxdb /home/backup/restore/influxdb

    2. Stop the Telegraf service to prevent database recreation:
       service telegraf stop

    3. Drop the existing `telegraf` database (as root or a privileged user):
       influx -execute 'DROP DATABASE telegraf'

    4. Restore the database from the backup:
       influxd restore -portable -db telegraf /home/backup/restore/influxdb

    5. Restart the Telegraf service:
       service telegraf start

Verify the result:
    - Check if the `telegraf` database is restored:
      influx -execute 'SHOW DATABASES'
    - Ensure Telegraf service is running:
      service telegraf status
