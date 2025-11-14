# Docker-based Jenkins Setup

This repository contains a Docker-based Jenkins setup for easy deployment and management.

## Prerequisites

- Docker Engine 20.10 or later
- Docker Compose 1.29 or later

## Quick Start

1. **Start Jenkins Server**
   ```bash
   docker-compose up -d
   ```

2. **Access Jenkins**
   - Open your browser and navigate to `http://localhost:8080`
   - Get the initial admin password:
     ```bash
     docker exec jenkins-server cat /var/jenkins_home/secrets/initialAdminPassword
     ```
   - Copy the password and paste it into the Jenkins setup wizard

3. **Complete Setup**
   - Follow the Jenkins setup wizard
   - The Git plugin is already pre-installed
   - Install additional plugins if needed
   - Create your first admin user

## Configuration

### Ports
- **8080**: Jenkins web interface
- **50000**: Jenkins agent communication port

### Volumes
- `jenkins_home`: Persistent storage for Jenkins data, configurations, and plugins

### Docker Socket
The Docker socket is mounted to allow Jenkins to run Docker commands (useful for Docker-in-Docker scenarios).

### Pre-installed Plugins
- **Git Plugin**: Pre-installed for Git integration in Jenkins jobs

## Useful Commands

### Start Jenkins
```bash
docker-compose up -d
```

### Stop Jenkins
```bash
docker-compose down
```

### View Logs
```bash
docker-compose logs -f jenkins
```

### Access Jenkins Container Shell
```bash
docker exec -it jenkins-server bash
```

### Backup Jenkins Data
```bash
docker run --rm -v jenkins-server_jenkins_home:/data -v $(pwd):/backup alpine tar czf /backup/jenkins-backup.tar.gz -C /data .
```

### Restore Jenkins Data
```bash
docker run --rm -v jenkins-server_jenkins_home:/data -v $(pwd):/backup alpine sh -c "cd /data && tar xzf /backup/jenkins-backup.tar.gz"
```

## Customization

### Pre-installed Components
- **Git Plugin**: Installed automatically via `plugins.txt`
- **Git CLI**: Installed in the container for Git operations
- **Docker CLI**: Installed for Docker-in-Docker scenarios

### Adding More Plugins
To add additional plugins:

1. Edit `plugins.txt` and add plugin names (one per line):
   ```
   git:latest
   pipeline-stage-view:latest
   blueocean:latest
   ```

2. Rebuild the Jenkins image:
   ```bash
   docker-compose build
   docker-compose up -d
   ```

Alternatively, you can install plugins via Jenkins UI: Manage Jenkins → Manage Plugins

## Troubleshooting

### Permission Issues
If you encounter permission issues with Docker socket, ensure your user is in the `docker` group:
```bash
sudo usermod -aG docker $USER
```

### Port Already in Use
If port 8080 is already in use, modify the port mapping in `docker-compose.yml`:
```yaml
ports:
  - "8081:8080"  # Use port 8081 instead
```

### Reset Jenkins
To completely reset Jenkins (⚠️ This will delete all data):
```bash
docker-compose down -v
docker-compose up -d
```

## Security Notes

- Change the default admin password after first login
- Consider setting up proper authentication (LDAP, OAuth, etc.)
- Review and restrict network access as needed
- Regularly update the Jenkins image to get security patches

## Additional Resources

- [Jenkins Official Documentation](https://www.jenkins.io/doc/)
- [Jenkins Docker Hub](https://hub.docker.com/r/jenkins/jenkins)
- [Docker Compose Documentation](https://docs.docker.com/compose/)

