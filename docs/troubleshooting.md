# Troubleshooting Runbook
Guide for general troubleshooting


# Helpful commands

<h2> Proxmox </h2>

```bash
# Check Battery life, charging status 
 acpi
```

<h2> Application </h2>

```bash
# Start the wiki app
# Navigate to /var/www/wiki
node server
```

<h2> Postgres DB </h2>

```bash
# Access Postgres CLI
su -u postgres -c psql
```



<h2> General Networking </h2>

```bash
# Show current routing table
ip route show

```

<h2> General Admin </h2>

```bash
# Check service status
systemctl status

# Check for service in logs
journalctl -u serviceName -b

```
