# Nginx Reverse Proxy Assignment

## 📋 Assignment Overview

This assignment demonstrates practical understanding of nginx reverse proxy configuration and web server management. Students will set up a complete nginx reverse proxy environment serving multiple web applications.

## 🎯 Learning Objectives

By completing this assignment, students will:
- Understand nginx reverse proxy concepts and configuration
- Learn to serve multiple web applications through a single nginx instance
- Practice Ubuntu server administration and file management
- Gain experience with web server security headers and optimization
- Work with real-world web application deployment scenarios

## 📁 Project Components

### Web Applications
1. **Budget Tracker** (`/budget`) - Personal finance management application
2. **Home Roster** (`/roster`) - Household chore management system
3. **Landing Page** (`/`) - Main entry point with navigation

### Technical Stack
- **Web Server**: Nginx
- **Operating System**: Ubuntu 20.04 LTS or later
- **Frontend**: HTML, CSS, JavaScript (Vanilla)
- **Configuration**: Nginx reverse proxy with location-based routing

## 📝 Required Deliverables

### 1. GitHub Repository
- [ ] Create a public GitHub repository
- [ ] Include all project files (HTML, CSS, JS, nginx.conf)
- [ ] Add comprehensive README.md with installation instructions
- [ ] Include this assignment.md file
- [ ] Ensure clean commit history with meaningful commit messages

### 2. Screenshots Documentation
- [ ] **Screenshot 1**: Main landing page (`http://YOUR_IP/`)
- [ ] **Screenshot 2**: Budget Tracker application (`http://YOUR_IP/budget`)
- [ ] **Screenshot 3**: Home Roster application (`http://YOUR_IP/roster`)
- [ ] **Screenshot 4**: Health check endpoint (`http://YOUR_IP/health`)
- [ ] **Screenshot 5**: Nginx configuration file (`/etc/nginx/sites-available/default`)
- [ ] **Screenshot 6**: Server file structure (`ls -la /usr/share/nginx/html/`)
- [ ] **Screenshot 7**: Nginx service status (`sudo systemctl status nginx`)

### 3. Configuration Verification
- [ ] **Test 1**: All applications accessible via reverse proxy paths
- [ ] **Test 2**: Health check endpoint returns "healthy"
- [ ] **Test 3**: Nginx configuration test passes (`sudo nginx -t`)
- [ ] **Test 4**: Firewall properly configured (port 80 open)
- [ ] **Test 5**: Applications work from external IP address

### 4. Technical Documentation
- [ ] **Documentation**: Complete README.md with step-by-step installation
- [ ] **Comments**: Well-commented nginx configuration file
- [ ] **Troubleshooting**: Document any issues encountered and solutions
- [ ] **Performance**: Explain nginx optimizations implemented (gzip, caching, etc.)

## 🔧 Technical Requirements

### Server Setup
- Ubuntu 20.04 LTS or later
- Nginx web server installed and configured
- Proper file permissions and ownership
- Firewall configured for HTTP traffic

### Nginx Configuration
- Reverse proxy setup for multiple applications
- Security headers implementation
- Gzip compression enabled
- Static file caching configured
- Health check endpoint available

### Application Features
- **Budget Tracker**: Income/expense tracking with categories
- **Home Roster**: Chore management with occupant assignment
- **Responsive Design**: Works on desktop and mobile devices
- **Interactive Elements**: Forms, filtering, task completion

## 📊 Evaluation Criteria

### Technical Implementation (40%)
- [ ] Nginx reverse proxy correctly configured
- [ ] All applications accessible via proper paths
- [ ] Security headers properly implemented
- [ ] Performance optimizations applied

### Documentation Quality (25%)
- [ ] Clear, step-by-step installation instructions
- [ ] Comprehensive README.md
- [ ] Well-commented configuration files
- [ ] Troubleshooting guide included

### Application Functionality (20%)
- [ ] Both web applications fully functional
- [ ] Interactive features working properly
- [ ] Responsive design implemented
- [ ] User experience is intuitive

### Screenshots & Verification (15%)
- [ ] All required screenshots provided
- [ ] Screenshots clearly show working applications
- [ ] Configuration verification completed
- [ ] External access confirmed

## 🚀 Bonus Points

### Advanced Features (Optional)
- [ ] **SSL/HTTPS Setup**: Implement Let's Encrypt SSL certificates
- [ ] **Custom Domain**: Configure a custom domain name
- [ ] **Load Balancing**: Add multiple backend servers
- [ ] **Monitoring**: Implement nginx status monitoring
- [ ] **Backup Strategy**: Document backup and recovery procedures

### Code Quality (Optional)
- [ ] **Error Handling**: Implement proper error pages
- [ ] **Logging**: Configure nginx access and error logs
- [ ] **Security**: Additional security measures beyond headers
- [ ] **Automation**: Scripts for deployment and maintenance

## 📅 Submission Guidelines

### Due Date
- **Submission Deadline**: [Insert due date]
- **Late Policy**: [Insert late submission policy]

### Submission Format
1. **GitHub Repository**: Public repository with all code and documentation
2. **Screenshots**: Upload to GitHub repository in `/screenshots/` folder
3. **Documentation**: Complete README.md and assignment.md files
4. **Configuration**: Include nginx.conf and any other config files

### File Organization
```
your-repo/
├── budget-app/
├── roster-app/
├── index.html
├── nginx.conf
├── README.md
├── assignment.md
└── screenshots/
    ├── landing-page.png
    ├── budget-app.png
    ├── roster-app.png
    ├── health-check.png
    ├── nginx-config.png
    ├── file-structure.png
    └── nginx-status.png
```

## 🆘 Getting Help

### Resources
- [Nginx Documentation](https://nginx.org/en/docs/)
- [Ubuntu Server Guide](https://ubuntu.com/server/docs)
- [Let's Encrypt Documentation](https://letsencrypt.org/docs/)

### Support
- **Office Hours**: [Insert office hours]
- **Discussion Forum**: [Insert forum link]
- **Email**: [Insert instructor email]

## ✅ Checklist

Before submitting, ensure you have:
- [ ] All required files in GitHub repository
- [ ] All screenshots taken and uploaded
- [ ] README.md is complete and accurate
- [ ] Nginx configuration is working properly
- [ ] All applications accessible externally
- [ ] Health check endpoint responding
- [ ] Code is well-commented and organized
- [ ] No sensitive information in repository (IPs, passwords, etc.)

---

**Good luck with your nginx reverse proxy implementation! 🚀**
