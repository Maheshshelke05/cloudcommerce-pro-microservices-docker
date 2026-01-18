# 🚀 Amazon Linux Deployment Guide (13.233.143.170)

## 📋 Amazon Linux Specific Commands

### Step 1: Connect to Your Server
```bash
# Connect to your Amazon Linux server
ssh -i your-key.pem ec2-user@13.233.143.170
```

### Step 2: Update Amazon Linux System
```bash
# Update all packages (Amazon Linux specific)
sudo yum update -y

# Check system info
cat /etc/os-release
# Should show: Amazon Linux 2
```

### Step 3: Install Docker on Amazon Linux
```bash
# Install Docker (Amazon Linux repository)
sudo yum install -y docker

# Start Docker service
sudo systemctl start docker

# Enable Docker to start on boot
sudo systemctl enable docker

# Check Docker status
sudo systemctl status docker
# Should show: active (running)

# Add ec2-user to docker group (no sudo needed)
sudo usermod -a -G docker ec2-user

# Verify Docker installation
docker --version
```

### Step 4: Install Docker Compose on Amazon Linux
```bash
# Download Docker Compose for Amazon Linux
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Make it executable
sudo chmod +x /usr/local/bin/docker-compose

# Create symlink for easier access
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

# Verify installation
docker-compose --version
```

### Step 5: Logout and Login (Important for Amazon Linux)
```bash
# Logout to apply group changes
exit

# Login again
ssh -i your-key.pem ec2-user@13.233.143.170

# Test Docker without sudo
docker ps
# Should work without permission error
```

### Step 6: Upload Project Files
```bash
# From your local machine:
scp -i your-key.pem -r . ec2-user@13.233.143.170:/home/ec2-user/ecommerce-app/
```

### Step 7: Deploy on Amazon Linux
```bash
# SSH back to server
ssh -i your-key.pem ec2-user@13.233.143.170

# Navigate to project
cd /home/ec2-user/ecommerce-app

# Check files uploaded
ls -la

# Start deployment (IP already configured: 13.233.143.170)
docker-compose -f docker-compose.prod.yml up -d

# Check container status
docker-compose -f docker-compose.prod.yml ps
```

## 🔧 Amazon Linux Specific Monitoring

### System Commands
```bash
# Check system resources (Amazon Linux)
free -h                    # Memory usage
df -h                      # Disk usage
top                        # CPU usage
sudo netstat -tulpn        # Network connections

# Check Docker service
sudo systemctl status docker

# Check logs
sudo journalctl -u docker -f
```

### Application Monitoring
```bash
# Check application health
curl http://localhost/health
curl http://13.233.143.170/health

# View container logs
docker-compose -f docker-compose.prod.yml logs -f

# Monitor container resources
docker stats
```

## 🛠️ Amazon Linux Troubleshooting

### Common Amazon Linux Issues

#### Issue 1: Docker Permission Denied
```bash
# If you get permission denied:
sudo usermod -a -G docker ec2-user
exit
ssh -i your-key.pem ec2-user@13.233.143.170
```

#### Issue 2: Docker Compose Not Found
```bash
# Reinstall Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

#### Issue 3: Port 80 Access Issues
```bash
# Check if port 80 is available
sudo netstat -tulpn | grep :80

# Check security group in AWS Console
# Ensure port 80 is open to 0.0.0.0/0
```

#### Issue 4: Out of Space (Amazon Linux)
```bash
# Check disk usage
df -h

# Clean up Docker (Amazon Linux safe)
docker system prune -a -f

# Clean up yum cache
sudo yum clean all
```

## 📦 Amazon Linux Package Management

### Useful Amazon Linux Commands
```bash
# Search for packages
yum search docker

# List installed packages
yum list installed | grep docker

# Update specific package
sudo yum update docker

# Install additional tools
sudo yum install -y htop git wget curl
```

## 🔐 Amazon Linux Security

### Firewall Configuration
```bash
# Check firewall status (Amazon Linux)
sudo systemctl status iptables

# If firewall is active, allow port 80
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo service iptables save
```

### System Updates
```bash
# Regular security updates
sudo yum update -y --security

# Check for available updates
yum check-update
```

## 🚀 Quick Deployment Commands (Copy-Paste Ready)

```bash
# 1. Connect to server
ssh -i your-key.pem ec2-user@13.233.143.170

# 2. Install Docker & Docker Compose
sudo yum update -y
sudo yum install -y docker
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -a -G docker ec2-user
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
exit

# 3. Upload files (from local machine)
scp -i your-key.pem -r . ec2-user@13.233.143.170:/home/ec2-user/ecommerce-app/

# 4. Deploy application
ssh -i your-key.pem ec2-user@13.233.143.170
cd /home/ec2-user/ecommerce-app
docker-compose -f docker-compose.prod.yml up -d

# 5. Verify deployment
docker-compose -f docker-compose.prod.yml ps
curl http://13.233.143.170/health
```

## 🌐 Access Your Application

- **Main Application**: http://13.233.143.170
- **Health Check**: http://13.233.143.170/health
- **API Endpoints**: http://13.233.143.170/api/products

## ✅ Success Checklist for Amazon Linux

- [ ] Amazon Linux 2 instance running
- [ ] Docker installed and running
- [ ] Docker Compose installed
- [ ] ec2-user added to docker group
- [ ] Project files uploaded
- [ ] All containers running
- [ ] Application accessible on port 80
- [ ] Health check returning OK

## 📞 Amazon Linux Support Commands

```bash
# System information
cat /etc/os-release
uname -a
whoami
groups

# Docker information
docker info
docker version
docker-compose version

# Network information
ip addr show
curl ifconfig.me  # Shows public IP
```

**🎉 Your Amazon Linux deployment is ready!**