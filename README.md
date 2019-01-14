# Disable THP on Linux machines to ensure best performance with MongoDB

### Add the script ###

(1) Move the file named `disable-transparent-hugepages` to `/usr/bin/`

(2) Ensure ownership of `root:root` for the file

(3) Ensure permissions of `0755` for the file

### Add the service ###

(4) Copy the file named `disable-transparent-hugepages.service` to `/etc/systemd/system/`

(5) Ensure ownership of `root:root` for the file

(6) Ensure permissions of `0644` for the file

(7) Run `systemctl daemon-reload`

(8) Run `systemctl enable disable-transparent-hugepages`

(9) Run `systemctl start disable-transparent-hugepages`

### Set THP ###

(9) Create a new path with `sudo mkdir /etc/tuned/no-thp/`

(10) Copy the file named `tuned.conf` to `/etc/tuned/no-thp/`

(11) Run `tuned-adm profile no-thp`

### Testing ###

(12) Run `cat /sys/kernel/mm/transparent_hugepage/enabled`

(13) Run `cat /sys/kernel/mm/transparent_hugepage/defrag`

(14) Both commands should produce this: `always madvise [never]`

# Rollback MongoDB from 4.0.5 to 3.6.9

Pre-req: shut down all LAs

(1) Check current featureCompatibility version (This may return a blank value)

`db.adminCommand( { getParameter: 1, featureCompatibilityVersion: 1 } )`

(2) Run `db.adminCommand({setFeatureCompatibilityVersion: "3.6"})`

(3) Check it again for sanity: `db.adminCommand( { getParameter: 1, featureCompatibilityVersion: 1 } )`

(4) Shut down mongo: `sudo service mongod stop`

(5) Check that mongo is shut down: `systemctl status mongod`

(6) Uninstall Mongo 4.0.5

`rpm -qa | grep mongo`

`sudo rpm -ev mongodb-org-4.0.5-1.el7.x86_64`
`sudo rpm -ev mongodb-org-mongos-4.0.5-1.el7.x86_64`
`sudo rpm -ev mongodb-org-server-4.0.5-1.el7.x86_64`
`sudo rpm -ev mongodb-org-shell-4.0.5-1.el7.x86_64`
`sudo rpm -ev mongodb-org-tools-4.0.5-1.el7.x86_64`

Check that all packages are removed: `rpm -qa | grep mongo`

(7) Install 3.6.9. Download all packages and run: `sudo rpm -i mongodb-org*.rpm`

