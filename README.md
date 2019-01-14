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

(9) Create a new path with `sudo mkdir /etc/tuned/no-thp`

(10) Copy the file named `tuned.conf` to `/etc/tuned/no-thp/`

(11) Run `tuned-adm profile no-thp`
