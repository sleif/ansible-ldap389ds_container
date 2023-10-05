# LDAP restore notes

## Prepare the backup ldif file for the import process

```sh
BACKUPFILE=openldap.data
mkdir -p /tmp/ldifs
cp backup/${BACKUPFILE}.gz /tmp/ldifs/backup.ldif.gz.orig
cd /tmp/ldifs
cp backup.ldif.gz.orig backup.ldif.gz
gzcat backup.ldif.gz.orig | sed -e ':a' -e 'N' -e '$!ba' -e 's/\n //g' | egrep -v  "^(structuralObjectClass|entryUUID|creatorsName|modifiersName|createTimestamp|modifyTimestamp|entryCSN|pwdChangedTime|memberOf|uniqueMember):" | gzip > backup.ldif.gz
```

- Run the ansible playbook with `-e ldap_initial_local_data_file=/tmp/ldifs/backup.ldif.gz`.
