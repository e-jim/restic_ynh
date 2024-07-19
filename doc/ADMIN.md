### Test

You can manually trigger backups using (as root or with sudo): `systemctl start restic.service`

And then make sure to verify the backup contents using (as root or with sudo): 
```
APP_NAME="restic"
BACKUP_TO_CHECK="auto_conf"

RESTIC_SERVER=$(yunohost app setting $APP_NAME server)
RESTIC_SERVER_PORT=$(yunohost app setting $APP_NAME port)
RESTIC_SERVER_USER=$(yunohost app setting $APP_NAME ssh_user)
RESTIC_PATH=$(yunohost app setting $APP_NAME backup_path)
RESTIC_PASSWORD="$(yunohost app setting $APP_NAME passphrase)"
RESTIC_REPOSITORY_BASE=sftp://$RESTIC_SERVER_USER@$RESTIC_SERVER:$RESTIC_SERVER_PORT/$RESTIC_PATH/

export RESTIC_PASSWORD
export RESTIC_REPOSITORY=${RESTIC_REPOSITORY_BASE}/$BACKUP_TO_CHECK

/var/www/restic/restic list snapshots
```

(Replace `auto_conf` with `auto_<app>` if you did not choose to backup configuration but only applications and `restic` with the name of the restic app, if you changed it or installed it multiple times (e.g. restic__2)

### Misc helpful commands

- Display the apps list to backup: `yunohost app setting restic apps`
- Edit the apps list to backup: `yunohost app setting restic apps -v "nextcloud,wordpress"`
- Launch a backup manually: `systemctl start restic`
- Launch a backup consistency check: `systemctl start restic_check.service`

##### Launch a complete backup check


If you want to make a complete check of the backups - keep in mind that this reads all the backed up data, it can take some time depending on your target server upload speed (more on this topic in [the Restic documentation](https://restic.readthedocs.io/en/latest/045_working_with_repos.html#checking-integrity-and-consistency)):

```
systemctl start restic_check_read_data.service
```
