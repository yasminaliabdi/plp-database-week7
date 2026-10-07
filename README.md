# Backups, PITR, and Replication Lab

## What I Did

- **Step 1:** Took a logical backup of the `bootcamp` database using `pg_dump` and verified it with `pg_restore`.
- **Step 2:** Enabled WAL archiving (`wal_level = replica`, `archive_mode = on`, `archive_command`).
- **Step 3:** Took a base backup using `pg_basebackup`.
- **Step 4:** Simulated a disaster by deleting all rows from `students`, then recovered using Point-in-Time Recovery (PITR) to `2026-10-06 22:41:18`.
- **Step 5:** Built a streaming standby replica using `pg_basebackup -R`.

## Screenshots

| File | Description |
|------|-------------|
| Screenshot 1 | Logical backup verification |
| Screenshot 2 | WAL archiving configuration |
| Screenshot 3 | Base backup completion |
| Screenshot 4 | Archived WAL files |
| Screenshot 5 | PITR recovery (Alice restored) |
| Screenshot 6 | Standby configuration and signal file |
| Screenshot 7 | Replication status check |

## Key Commands Used

```bash
# Logical backup
pg_dump -Fc -f /tmp/bootcamp.dump bootcamp

# WAL archiving config (in postgresql.conf)
wal_level = replica
archive_mode = on
archive_command = 'cp %p /tmp/backups/wal/%f'

# Base backup
pg_basebackup -D /tmp/backups/base -Ft -z -Xs -P

# PITR recovery
restore_command = 'cp /tmp/backups/wal/%f %p'
recovery_target_time = '2026-10-06 22:41:18'

# Streaming standby
pg_basebackup -h 127.0.0.1 -U replicator -D /tmp/backups/standby -R -P
