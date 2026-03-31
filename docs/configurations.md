# Configurations
<i> Various code snippets for optimizing this project. </i>


# Proxmox
```bash
# Check Sleep status on device
# Loaded & Inactive mean it is able to be used
systemctl status sleep.target suspend.target hibernate.target hybrid-sleep.target

# Ensure Sleep status is not enabled
systemctl status sleep.target suspend.target hibernate.target hybrid-sleep.target
```
