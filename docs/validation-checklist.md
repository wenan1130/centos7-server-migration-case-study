# CentOS 7 Server Migration Validation Checklist

This checklist is used to validate a migrated CentOS 7 physical server before the migration is considered complete.

The goal is not only to confirm that the operating system boots, but also to verify storage, networking, services, applications, data, backup, and rollback readiness.

---

## 1. System Boot Validation

- [ ] Server powers on normally
- [ ] BIOS / UEFI detects required hardware
- [ ] RAID controller reports expected virtual disk status
- [ ] Boot partition is accessible
- [ ] GRUB loads successfully
- [ ] Expected kernel is available
- [ ] CentOS 7 boots without emergency mode
- [ ] Root filesystem mounts successfully
- [ ] No unexpected filesystem recovery errors are present

---

## 2. Storage Validation

- [ ] Target disk size matches the migration plan
- [ ] Partition table is correct
- [ ] `/boot` partition is present and mountable
- [ ] LVM physical volume is detected
- [ ] Expected volume group is available
- [ ] Expected logical volumes are available
- [ ] Logical volume sizes match the target design
- [ ] Filesystem types are correct
- [ ] Filesystems mount without errors
- [ ] `/etc/fstab` entries are valid
- [ ] Additional storage capacity is allocated as planned

Suggested verification commands:

```bash
lsblk
blkid
pvs
vgs
lvs
df -h
mount
cat /etc/fstab

3. Filesystem and Data Validation
- [ ] Root filesystem content is present
- [ ] Application directories are present
- [ ] User data is present
- [ ] File ownership is preserved
- [ ] File permissions are preserved
- [ ] Symbolic links are preserved
- [ ] Important configuration files are present
- [ ] Required logs and application data are accessible
4. Network Validation
- [ ] Expected network interfaces are detected
- [ ] Interface naming is correct
- [ ] IP configuration is correct
- [ ] Default gateway is correct
- [ ] DNS configuration is correct
- [ ] Hostname is correct
- [ ] Required static routes are present
- [ ] Local gateway is reachable
- [ ] Required internal systems are reachable
- [ ] Required external destinations are reachable
Suggested verification commands:
ip addr
ip route
hostname
cat /etc/resolv.conf
ping -c 3 <gateway>

Do not publish real production IP addresses or internal hostnames in public documentation.
5. System Service Validation
- [ ] Required services start successfully
- [ ] Required services are enabled as expected
- [ ] No critical required service is failed
- [ ] Service dependencies are available
- [ ] Application-related daemons are running
Suggested verification commands:
systemctl --failed
systemctl list-units --type=service --state=running

6. Application Validation
- [ ] Application starts successfully
- [ ] Application processes are running
- [ ] Application configuration is present
- [ ] Required ports are listening
- [ ] Application can access required data
- [ ] Application can access required backend services
- [ ] Application user accounts and permissions are correct
- [ ] Basic functional test passes
- [ ] Application behavior matches the source server baseline
Suggested verification commands:
ps -ef
ss -lntp

7. Database Validation
If the migrated system contains or depends on a database:
- [ ] Database service starts successfully
- [ ] Database files are accessible
- [ ] Required databases are present
- [ ] Application can connect to the database
- [ ] Basic read test succeeds
- [ ] Expected tables or schemas are available
- [ ] Data appears consistent with the source baseline
Avoid publishing production credentials, connection strings, or customer data.
8. Security Validation
- [ ] Required user accounts are present
- [ ] Required groups are present
- [ ] File permissions remain correct
- [ ] SSH configuration is preserved as required
- [ ] Firewall configuration is reviewed
- [ ] SELinux state matches the expected baseline
- [ ] No credentials were unintentionally exposed during migration
- [ ] Private keys remain protected
Suggested verification commands:
getenforce
firewall-cmd --state
sshd -T

9. Backup and Rollback Validation
- [ ] Original server remains unchanged until final acceptance
- [ ] Full migration backup is retained
- [ ] Backup location is documented
- [ ] Backup integrity has been checked
- [ ] Rollback procedure is documented
- [ ] Rollback decision point is defined
- [ ] Original production server can be restored to service if required
10. Performance and Health Validation
- [ ] CPU utilization is within expected range
- [ ] Memory usage is within expected range
- [ ] Disk utilization is within expected range
- [ ] No unexpected I/O errors are present
- [ ] System logs show no critical migration-related errors
- [ ] Application response time is acceptable
Suggested verification commands:
uptime
free -m
df -h
dmesg
journalctl -p err

11. Final Acceptance Gate
Migration should only be considered complete when:
- [ ] System boot validation passes
- [ ] Storage validation passes
- [ ] Network validation passes
- [ ] Required services are healthy
- [ ] Application validation passes
- [ ] Data validation passes
- [ ] Backup and rollback readiness are confirmed
- [ ] No unresolved critical issue remains

12. Validation Result
Migration Validation Result:

[ ] PASS
[ ] PASS WITH OBSERVATIONS
[ ] FAIL

Validated by:
Date:
Target server:
Migration reference:
Open issues:

Documentation Principle
A successful migration is not defined only by whether the Linux server can boot.
The migrated system must also demonstrate that:
- Storage is correct
- Networking is functional
- Services are available
- Applications work
- Data is intact
- Rollback remains possible
Only then should the migration be formally accepted.
