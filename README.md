
# Portainer Password Reset Guide (Docker Swarm)

This guide outlines the standard procedure for resetting the admin password of a Portainer instance running as a Docker Swarm service. 

**Important:** You must scale down the Portainer service before running the reset helper to prevent database locking issues.

## Step-by-Step Instructions

### 1. Stop the Portainer Service
Scale the service down to `0`. This ensures Docker Swarm does not attempt to restart the container while you are resetting the password, which would lock the database file.

```
docker service scale portainer_portainer=0
```

### 2. Run the Password Reset Helper
Run the official Portainer password reset helper tool. Ensure you attach the correct volume (`portainer_portainer_data`) where your active Portainer database is stored.

```
docker run --rm -v portainer_portainer_data:/data portainer/helper-reset-password
```

**Note:** If successful, the console will output a new temporary password. Copy this password immediately.

## 3. Restart the Portainer Service

Bring the Portainer service back online by scaling it back to 1.

```
docker service scale portainer_portainer=1
```

### Next Steps
Once the service is fully running, access your Portainer web interface. Log in using the username `admin` and the temporary password generated in Step 2. Navigate to your account settings to configure a new, secure permanent password.
