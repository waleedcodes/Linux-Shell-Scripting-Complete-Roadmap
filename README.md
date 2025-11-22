# 🐧 The Ultimate Linux + Shell Scripting Mastery Guide

> **A complete, practical, beginner-friendly roadmap for Linux + Shell Scripting**  
> Specially tailored for developers transitioning to backend/server/DevOps skills

---

## 📋 Table of Contents

- [Why This Matters](#-why-this-matters)
- [Learning Timeline](#-learning-timeline)
- [Phase 1: Linux Foundations](#-phase-1-linux-foundations-weeks-1-2)
- [Phase 2: System Administration](#-phase-2-system-administration-weeks-3-4)
- [Phase 3: Shell Scripting Basics](#-phase-3-shell-scripting-basics-weeks-5-6)
- [Phase 4: Advanced Scripting](#-phase-4-advanced-scripting-weeks-7-8)
- [Phase 5: DevOps Integration](#-phase-5-devops-integration-weeks-9-12)
- [Real-World Projects](#-real-world-projects)
- [Resources](#-best-resources)
- [Practice Challenges](#-daily-practice-challenges)

---

## 🎯 Why This Matters

As a web developer, mastering Linux + Shell Scripting unlocks:

| Skill | Benefit |
|-------|---------|
| **Server Management** | Deploy MERN/Next.js apps on VPS (DigitalOcean, AWS, Linode) |
| **DevOps Automation** | Automate deployments, backups, monitoring |
| **Career Growth** | Backend/Full-stack roles expect Linux proficiency |
| **Problem Solving** | Debug production issues, optimize server performance |
| **Cost Efficiency** | Self-host apps, reduce hosting costs |

---

## 📅 Learning Timeline (Realistic)

| Phase | Duration | Focus | Skills Gained |
|-------|----------|-------|---------------|
| **Phase 1** | Weeks 1-2 | Linux fundamentals | Terminal fluency, file navigation |
| **Phase 2** | Weeks 3-4 | System administration | Users, permissions, processes, networking |
| **Phase 3** | Weeks 5-6 | Shell scripting foundations | Variables, loops, conditions, functions |
| **Phase 4** | Weeks 7-8 | Advanced scripting | Text processing, automation, error handling |
| **Phase 5** | Weeks 9-12 | DevOps integration | Deployment automation, monitoring, CI/CD |

**Daily commitment**: 1-2 hours (consistency beats intensity)

---

## 🔹 Phase 1: Linux Foundations (Weeks 1-2)

### Understanding the Filesystem

Linux organizes everything as files in a hierarchical tree:

```
/                    (root - everything starts here)
├── bin             (essential binaries: ls, cat, bash)
├── etc             (configuration: nginx.conf, ssh_config)
├── home            (user directories: /home/waleed)
│   └── waleed
├── var             (variable data: logs, databases, caches)
│   ├── log         (system logs)
│   └── www         (web server files)
├── usr             (user programs & libraries)
└── tmp             (temporary files - cleared on reboot)
```

**💡 Real-world context**: When deploying apps:
- Code lives in `/var/www` or `/home/user/app`
- Logs are in `/var/log/nginx` or `/var/log/app`
- Config files in `/etc/nginx` or `/etc/systemd`

### Essential Commands

#### Navigation & Exploration

```bash
pwd                 # Print working directory - where am I?
ls -lah            # List files (long format, all files, human-readable)
cd /var/log        # Change directory
cd ~               # Go to home directory
cd -               # Go back to previous directory
tree -L 2          # Visual directory tree (2 levels deep)
```

#### File Operations

```bash
# Creating
touch app.js                    # Create empty file
mkdir -p projects/backend       # Create nested directories (-p = parents)

# Viewing
cat package.json               # Display entire file
less server.log                # View large files (q to quit)
head -n 20 error.log          # First 20 lines
tail -f access.log            # Follow log in real-time ⭐ CRITICAL

# Editing
nano config.js                 # Beginner-friendly editor
vim server.js                  # Powerful editor (steep learning curve)

# Copying & Moving
cp app.js app.backup.js        # Copy file
cp -r frontend/ frontend-v2/   # Copy directory recursively
mv old.js new.js               # Rename/move file
mv *.log logs/                 # Move all .log files

# Deleting (CAREFUL!)
rm file.js                     # Delete file
rm -rf node_modules/           # Delete directory (recursive + force)
```

#### Searching & Filtering

```bash
# Find files
find . -name "*.js"                    # All JS files
find /var/log -name "error*" -mtime -7 # Modified in last 7 days
find . -type f -size +10M              # Files larger than 10MB

# Search text inside files
grep "error" server.log                # Find lines with "error"
grep -r "TODO" ./src                  # Recursive search
grep -i "warning" *.log               # Case-insensitive
grep -n "function" app.js             # Show line numbers
grep -v "debug" log.txt               # Invert match (exclude "debug")
```

### 🎯 Practice Task 1: File System Explorer

```bash
# Create project structure
mkdir -p ~/dev-projects/{backend,frontend,scripts,docs}
cd ~/dev-projects

# Create files
touch backend/{server.js,db.js,auth.js}
touch frontend/{app.jsx,styles.css,index.html}
touch scripts/{deploy.sh,backup.sh}

# Add content
echo "console.log('Hello Linux')" > backend/server.js
echo "# Project Documentation" > docs/README.md

# Explore
tree
cat backend/server.js
find . -name "*.js"
grep -r "console.log" .
```

**Challenge**: Use `find` to locate all `.js` files and count them using `wc -l`

---

## 🔹 Phase 2: System Administration (Weeks 3-4)

### Understanding Permissions

Every file has permissions for **User**, **Group**, **Others**:

```bash
ls -l app.js
# Output: -rw-r--r-- 1 waleed waleed 1024 Nov 22 10:30 app.js
#         │││ │││ │││
#         Owner Group Others
```

**Permission breakdown**:
- `r` (read = 4): View file content
- `w` (write = 2): Modify file
- `x` (execute = 1): Run as program

**Numeric notation**: 
- `755` = `rwxr-xr-x` (owner: full, others: read+execute)
- `644` = `rw-r--r--` (owner: read+write, others: read-only)
- `700` = `rwx------` (owner only)

#### Changing Permissions

```bash
chmod 755 deploy.sh          # Numeric: Owner rwx, Group/Others rx
chmod +x script.sh           # Add execute permission
chmod u+w,g-w file.txt      # User add write, Group remove write

# Common patterns
chmod 644 *.json            # Config files (owner rw, others read)
chmod 755 *.sh              # Scripts (executable)
chmod 700 secrets.env       # Private files (owner only)
chmod -R 755 /var/www       # Recursive (-R for directories)

# Change ownership
sudo chown waleed:waleed app.js      # Change owner and group
sudo chown -R www-data:www-data /var/www  # Web server ownership
```

### Process Management

Every running program is a "process":

```bash
# View processes
ps aux                        # All processes
ps aux | grep node           # Find Node.js processes
top                          # Interactive viewer (press q to quit)
htop                         # Better version (install: sudo apt install htop)

# Managing processes
kill 1234                    # Gracefully stop process ID 1234
kill -9 1234                 # Force kill (SIGKILL - last resort)
pkill node                   # Kill by name
killall -9 node              # Force kill all Node processes

# Background processes
node server.js &             # Run in background
nohup node server.js &       # Keep running after logout
jobs                         # List background jobs
fg %1                        # Bring job 1 to foreground
bg %1                        # Resume job 1 in background

# Process monitoring
watch -n 2 'ps aux | grep node'  # Refresh every 2 seconds
```

**Real scenario**: Node.js server stuck? Kill and restart:
```bash
ps aux | grep node           # Find PID (e.g., 5432)
kill -9 5432                # Force stop
node server.js &            # Restart in background
```

### System Services (systemd)

Modern Linux uses `systemd` to manage services:

```bash
# Service management
sudo systemctl start nginx      # Start service
sudo systemctl stop nginx       # Stop service
sudo systemctl restart nginx    # Restart
sudo systemctl reload nginx     # Reload config without stopping
sudo systemctl status nginx     # Check status

# Enable/disable auto-start
sudo systemctl enable nginx     # Start on boot
sudo systemctl disable nginx    # Don't start on boot

# View logs
sudo journalctl -u nginx        # Service logs
sudo journalctl -f              # Follow all system logs
```

### Package Management

**Ubuntu/Debian (apt)**:
```bash
sudo apt update              # Update package list
sudo apt upgrade             # Upgrade all packages
sudo apt install nginx       # Install package
sudo apt remove nginx        # Uninstall
sudo apt autoremove          # Clean up unused dependencies
sudo apt search nodejs       # Search packages
```

**Fedora/RHEL (yum/dnf)**:
```bash
sudo yum install nodejs
sudo dnf install nginx
```

**Universal (Node.js ecosystem)**:
```bash
npm install -g pm2           # PM2 process manager
npm install -g nodemon       # Auto-restart dev server
```

### Networking Essentials

```bash
# Check connectivity
ping google.com              # Test internet (Ctrl+C to stop)
ping -c 4 8.8.8.8           # Ping 4 times then stop

# Download files
wget https://example.com/file.zip        # Download file
curl https://api.github.com/users/waleed # Fetch API data
curl -o output.json https://api.com/data # Save to file
curl -X POST -H "Content-Type: application/json" \
     -d '{"key":"value"}' https://api.com/endpoint  # POST request

# Network info
ip addr                      # Show IP addresses
netstat -tuln               # Show listening ports
ss -tuln                    # Modern alternative to netstat
lsof -i :3000               # What's using port 3000?

# SSH (connect to remote servers)
ssh username@server-ip                    # Connect
ssh root@142.93.150.20                   # Example
ssh -i ~/.ssh/key.pem ubuntu@ec2-host    # With private key
ssh -p 2222 user@host                    # Custom port

# Transfer files
scp file.txt user@server:/path/          # Upload to server
scp user@server:/path/file.txt .         # Download from server
scp -r folder/ user@server:/path/        # Upload directory
```

### Disk & System Info

```bash
# Disk usage
df -h                       # Disk space (human-readable)
du -sh *                    # Size of each item in current dir
du -h --max-depth=1 /var    # Size of subdirectories

# System info
uname -a                    # System information
lsb_release -a             # Ubuntu version
cat /etc/os-release        # OS details
free -h                    # Memory usage
uptime                     # How long system running
who                        # Who's logged in
```

### 🎯 Practice Task 2: Server Setup Simulation

```bash
# 1. Install web server
sudo apt update
sudo apt install nginx

# 2. Check status
sudo systemctl status nginx

# 3. Find process
ps aux | grep nginx

# 4. Test server
curl http://localhost        # Should show nginx welcome page

# 5. View logs
sudo tail -f /var/log/nginx/access.log

# 6. Make a test request in another terminal
curl http://localhost

# 7. Check port usage
sudo lsof -i :80

# 8. Disk usage
df -h
du -sh /var/log/nginx
```

---

## 🔹 Phase 3: Shell Scripting Basics (Weeks 5-6)

### Your First Script

Create `hello.sh`:
```bash
#!/bin/bash
# This is a comment - shebang tells system to use bash

echo "What's your name?"
read name
echo "Hello, $name! Welcome to shell scripting."
echo "Today is $(date +%A), $(date +%B %d, %Y)"
```

Make it executable and run:
```bash
chmod +x hello.sh
./hello.sh
```

### Variables & Data Types

```bash
#!/bin/bash

# Variables (NO SPACES around =)
name="Waleed"
age=25
is_developer=true

# Using variables
echo "Name: $name"
echo "Age: ${age} years old"  # {} for clarity

# Command output as variable
current_date=$(date)
files_count=$(ls | wc -l)
echo "Today is $current_date"
echo "Files in directory: $files_count"

# Math operations
num1=10
num2=5
sum=$((num1 + num2))
product=$((num1 * num2))
echo "Sum: $sum, Product: $product"

# String operations
full_name="$name Developer"
echo "Length: ${#name}"        # String length
echo "Uppercase: ${name^^}"    # Convert to uppercase
echo "Lowercase: ${name,,}"    # Convert to lowercase
```

### User Input

```bash
#!/bin/bash

# Simple input
read -p "Enter your name: " username
echo "Hello, $username"

# Silent input (for passwords)
read -sp "Enter password: " password
echo  # New line after silent input

# Multiple inputs
read -p "Enter first and last name: " first last
echo "First: $first, Last: $last"

# Default value
read -p "Enter environment [development]: " env
env=${env:-development}  # Use 'development' if empty
echo "Environment: $env"
```

### Conditional Logic

```bash
#!/bin/bash

read -p "Enter your age: " age

if [ $age -lt 18 ]; then
    echo "You're a minor"
elif [ $age -lt 60 ]; then
    echo "You're an adult"
else
    echo "You're a senior"
fi

# File checks
if [ -f "config.json" ]; then
    echo "✅ Config file exists"
else
    echo "❌ Config file missing!"
fi

# Directory check
if [ -d "node_modules" ]; then
    echo "Dependencies installed"
else
    echo "Run npm install first"
    npm install
fi

# String comparison
if [ "$NODE_ENV" == "production" ]; then
    echo "Running in production mode"
elif [ "$NODE_ENV" == "development" ]; then
    echo "Running in development mode"
else
    echo "Environment not set"
fi

# Multiple conditions (AND)
if [ -f "package.json" ] && [ -d "node_modules" ]; then
    echo "Node.js project ready"
fi

# Multiple conditions (OR)
if [ "$1" == "start" ] || [ "$1" == "run" ]; then
    echo "Starting server..."
fi
```

**Common test operators**:
- **Files**: `-f` (file exists), `-d` (directory), `-e` (exists), `-s` (not empty)
- **Numbers**: `-eq` (equal), `-ne` (not equal), `-lt` (less than), `-le` (less/equal), `-gt` (greater), `-ge` (greater/equal)
- **Strings**: `=` (equal), `!=` (not equal), `-z` (empty), `-n` (not empty)
- **Logic**: `&&` (AND), `||` (OR), `!` (NOT)

### Loops

```bash
#!/bin/bash

# For loop - iterate over list
for file in *.js; do
    echo "Processing $file"
    # Count lines
    lines=$(wc -l < "$file")
    echo "  Lines: $lines"
done

# For loop - counter
for i in {1..5}; do
    echo "Iteration $i"
done

# For loop - C-style
for ((i=1; i<=5; i++)); do
    echo "Count: $i"
done

# While loop
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done

# Read file line by line
while IFS= read -r line; do
    echo "Line: $line"
done < input.txt

# Infinite loop (use Ctrl+C to stop)
while true; do
    echo "Running..."
    sleep 2
done
```

### Functions

```bash
#!/bin/bash

# Define function
greet() {
    local name=$1  # $1 is first argument, local = function scope
    echo "Hello, $name!"
}

# Call function
greet "Waleed"
greet "Ahmed"

# Function with return value
calculate_sum() {
    local num1=$1
    local num2=$2
    local sum=$((num1 + num2))
    echo $sum  # "Return" by echoing
}

result=$(calculate_sum 10 20)
echo "Sum is: $result"

# Function with multiple outputs
get_file_info() {
    local file=$1
    echo "File: $file"
    echo "Size: $(stat -f%z "$file" 2>/dev/null || stat -c%s "$file") bytes"
    echo "Modified: $(stat -f%Sm "$file" 2>/dev/null || stat -c%y "$file")"
}

get_file_info "script.sh"

# Function with exit code
check_service() {
    if systemctl is-active --quiet "$1"; then
        return 0  # Success
    else
        return 1  # Failure
    fi
}

if check_service nginx; then
    echo "✅ Nginx is running"
else
    echo "❌ Nginx is not running"
fi
```

### Arrays

```bash
#!/bin/bash

# Declare array
fruits=("apple" "banana" "cherry")
numbers=(1 2 3 4 5)

# Access elements
echo "First fruit: ${fruits[0]}"
echo "Second fruit: ${fruits[1]}"

# All elements
echo "All fruits: ${fruits[@]}"

# Array length
echo "Number of fruits: ${#fruits[@]}"

# Loop through array
for fruit in "${fruits[@]}"; do
    echo "Fruit: $fruit"
done

# Add element
fruits+=("date")

# Associative arrays (key-value pairs) - Bash 4+
declare -A config
config[host]="localhost"
config[port]="3000"
config[env]="production"

echo "Host: ${config[host]}"
echo "Port: ${config[port]}"
```

### 🎯 Practice Task 3: Project Initializer

Create `init-project.sh`:
```bash
#!/bin/bash

echo "🚀 Project Initializer"
echo "====================="

# Get project details
read -p "Enter project name: " project_name
read -p "Enter project type (node/python/react): " project_type

# Validate input
if [ -z "$project_name" ]; then
    echo "❌ Project name cannot be empty!"
    exit 1
fi

if [ -d "$project_name" ]; then
    echo "❌ Project '$project_name' already exists!"
    exit 1
fi

# Create structure
echo "📁 Creating project structure..."
mkdir -p "$project_name"/{src,tests,docs}
cd "$project_name"

# Project-specific setup
case $project_type in
    node)
        touch src/index.js
        echo '{"name": "'$project_name'", "version": "1.0.0"}' > package.json
        echo "node_modules/\n.env" > .gitignore
        ;;
    python)
        touch src/main.py
        touch requirements.txt
        echo "__pycache__/\n*.pyc\nvenv/" > .gitignore
        ;;
    react)
        touch src/App.jsx
        touch src/index.css
        echo "node_modules/\nbuild/\n.env" > .gitignore
        ;;
    *)
        echo "⚠️  Unknown project type, creating generic structure"
        ;;
esac

# Common files
touch tests/test.js
touch README.md
echo "# $project_name" > README.md
echo "Created on $(date)" >> README.md

echo "✅ Project '$project_name' created successfully!"
echo ""
echo "📊 Project structure:"
tree -L 2

echo ""
echo "Next steps:"
echo "  cd $project_name"
echo "  git init"
echo "  # Start coding!"
```

Run it:
```bash
chmod +x init-project.sh
./init-project.sh
```

---

## 🔹 Phase 4: Advanced Scripting (Weeks 7-8)

### Command Line Arguments

```bash
#!/bin/bash

# Script: backup.sh - Backup files with arguments

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "All arguments: $@"
echo "Number of arguments: $#"

# Check if arguments provided
if [ $# -eq 0 ]; then
    echo "Usage: $0 <source> <destination>"
    echo "Example: $0 /var/www /backup"
    exit 1
fi

source_dir=$1
dest_dir=${2:-/backup}  # Default to /backup if not provided

echo "Backing up $source_dir to $dest_dir..."
cp -r "$source_dir" "$dest_dir"
echo "✅ Backup complete!"
```

### Text Processing Power Tools

#### grep - Search patterns
```bash
# Basic search
grep "error" server.log

# Case-insensitive
grep -i "error" server.log

# Show line numbers
grep -n "error" server.log

# Show context (3 lines before, 2 after)
grep -B 3 -A 2 "error" server.log

# Multiple patterns
grep -E "ERROR|WARNING|CRITICAL" server.log

# Invert match (exclude lines)
grep -v "debug" server.log

# Count matches
grep -c "error" server.log

# Recursive search
grep -r "TODO" ./src

# Search with regex
grep -E "^[0-9]{3}" log.txt  # Lines starting with 3 digits
```

#### sed - Stream editor (find & replace)
```bash
# Replace first occurrence per line
sed 's/old/new/' file.txt

# Replace all occurrences
sed 's/old/new/g' file.txt

# Replace in file (in-place)
sed -i 's/localhost/example.com/g' config.js

# Delete lines matching pattern
sed '/debug/d' log.txt

# Delete lines 5-10
sed '5,10d' file.txt

# Add line after match
sed '/pattern/a\New line here' file.txt

# Multiple operations
sed -e 's/foo/bar/g' -e 's/hello/world/g' file.txt

# Replace only on specific lines
sed '3,7s/old/new/g' file.txt
```

#### awk - Pattern scanning and processing
```bash
# Print specific columns (space-separated)
awk '{print $1, $3}' file.txt

# Print lines where column 3 > 100
awk '$3 > 100' data.txt

# Sum column 2
awk '{sum += $2} END {print sum}' numbers.txt

# CSV processing (comma-separated)
awk -F',' '{print $1, $3}' data.csv

# Calculate average
awk '{sum+=$1; count++} END {print sum/count}' numbers.txt

# Conditional printing
awk '$3 == "active" {print $1, $2}' users.txt

# Format output
awk '{printf "Name: %-10s Score: %d\n", $1, $2}' scores.txt
```

**Real-world example: Log analysis**
```bash
#!/bin/bash

# analyze-logs.sh - Analyze nginx access logs

log_file="/var/log/nginx/access.log"

echo "📊 Log Analysis Report"
echo "===================="

# Total requests
total=$(wc -l < "$log_file")
echo "Total requests: $total"

# Requests by status code
echo -e "\n📈 Status codes:"
awk '{print $9}' "$log_file" | sort | uniq -c | sort -rn

# Top 10 IPs
echo -e "\n🌐 Top 10 IP addresses:"
awk '{print $1}' "$log_file" | sort | uniq -c | sort -rn | head -10

# Most requested URLs
echo -e "\n🔗 Top 10 URLs:"
awk '{print $7}' "$log_file" | sort | uniq -c | sort -rn | head -10

# Errors (4xx, 5xx)
error_count=$(awk '$9 ~ /^[45]/ {print $0}' "$log_file" | wc -l)
echo -e "\n❌ Total errors (4xx/5xx): $error_count"

# Traffic by hour
echo -e "\n⏰ Requests by hour:"
awk '{print $4}' "$log_file" | cut -d: -f2 | sort | uniq -c
```

### Error Handling & Debugging

```bash
#!/bin/bash

# Enable error handling
set -e          # Exit on any error
set -u          # Exit on undefined variable
set -o pipefail # Exit if any pipe command fails

# Debug mode (print each command)
set -x

# Custom error handling
error_exit() {
    echo "❌ Error: $1" >&2
    exit 1
}

# Check if file exists
[ -f "config.json" ] || error_exit "config.json not found"

# Check if command succeeded
if ! npm install; then
    error_exit "npm install failed"
fi

# Try-catch equivalent
deploy_app() {
    if ! git pull origin main; then
        echo "⚠️  Git pull failed, using local version"
        return 1
    fi
    
    if ! npm install; then
        error_exit "Dependency installation failed"
    fi
    
    if ! npm run build; then
        error_exit "Build failed"
    fi
    
    echo "✅ Deployment successful"
}

deploy_app

# Log everything to file
exec > >(tee -a deploy.log)
exec 2>&1

echo "Deployment started at $(date)"
```

### Automation with Cron Jobs

Cron runs scheduled tasks automatically.

**Edit cron jobs**:
```bash
crontab -e      # Edit your cron jobs
crontab -l      # List current cron jobs
crontab -r      # Remove all cron jobs
```

**Cron syntax**:
```
* * * * * command
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, Sun=0 or 7)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

**Examples**:
```bash
# Every minute
* * * * * /path/to/script.sh

# Every hour at minute 0
0 * * * * /path/to/script.sh

# Every day at 2:30 AM
30 2 * * * /path/to/backup.sh

# Every Monday at 9 AM
0 9 * * 1 /path/to/weekly-report.sh

# Every 15 minutes
*/15 * * * * /path/to/check-status.sh

# First day of every month at midnight
0 0 1 * * /path/to/monthly-cleanup.sh

# Log output
0 2 * * * /path/to/backup.sh >> /var/log/backup.log 2>&1
```

### 🎯 Practice Task 4: Automated Backup Script

Create `auto-backup.sh`:
```bash
#!/bin/bash

# auto-backup.sh - Automated backup with rotation

set -e  # Exit on error

# Configuration
SOURCE_DIR="/var/www/myapp"
BACKUP_DIR="/backup"
RETENTION_DAYS=7
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_NAME="backup_${DATE}.tar.gz"
LOG_FILE="/var/log/backup.log"

# Function to log messages
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

log "🔄 Starting backup process"

# Check if source exists
if [ ! -d "$SOURCE_DIR" ]; then
    log "❌ Source directory not found: $SOURCE_DIR"
    exit 1
fi

# Create backup directory if needed
mkdir -p "$BACKUP_DIR"

# Create backup
log "📦 Creating backup: $BACKUP_NAME"
if tar -czf "${BACKUP_DIR}/${BACKUP_NAME}" -C "$SOURCE_DIR" . ; then
    log "✅ Backup created successfully"
    
    # Calculate size
    size=$(du -h "${BACKUP_DIR}/${BACKUP_NAME}" | cut -f1)
    log "📊 Backup size: $size"
else
    log "❌ Backup failed"
    exit 1
fi

# Delete old backups
log "🗑️  Removing backups older than $RETENTION_DAYS days"
find "$BACKUP_DIR" -name "backup_*.tar.gz" -mtime +$RETENTION_DAYS -delete

# Count remaining backups
backup_count=$(find "$BACKUP_DIR" -name "backup_*.tar.gz" | wc -l)
log "📚 Total backups: $backup_count"

log "✅ Backup process completed"

# Optional: Send notification
# echo "Backup completed: $BACKUP_NAME" | mail -s "Backup Success" admin@example.com
```

Make it executable and add to cron:
```bash
chmod +x auto-backup.sh

# Add to cron (run daily at 2 AM)
(crontab -l 2>/dev/null; echo "0 2 * * * /path/to/auto-backup.sh") | crontab -
```

---

## 🔹 Phase 5: DevOps Integration (Weeks 9-12)

### Environment Management

```bash
#!/bin/bash

# deploy.sh - Deployment script with environment handling

# Load environment variables
if [ -f .env ]; then
    export $(cat .env | grep -v '^#' | xargs)
else
    echo "❌ .env file not found"
    exit 1
fi

# Validate required variables
required_vars=("DB_HOST" "DB_USER" "API_KEY")
for var in "${required_vars[@]}"; do
    if [ -z "${!var}" ]; then
        echo "❌ Required variable $var is not set"
        exit 1
    fi
done

echo "✅ Environment variables validated"
echo "🚀 Deploying to $ENVIRONMENT environment..."

# Deployment logic
npm install --production
npm run build
pm2 restart app || pm2 start npm --name "app" -- start
```

### Git Integration & Automation

```bash
#!/bin/bash

# git-deploy.sh - Automated git deployment

set -e

PROJECT_DIR="/var/www/myapp"
BRANCH="main"
LOG_FILE="/var/log/deploy.log"

log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

log "🔄 Starting deployment from Git"

cd "$PROJECT_DIR"

# Check for uncommitted changes
if ! git diff-index --quiet HEAD --; then
    log "⚠️  Uncommitted changes detected, stashing..."
    git stash
fi

# Fetch latest changes
log "📥 Fetching latest changes..."
git fetch origin "$BRANCH"

# Get current and remote commit hashes
current_commit=$(git rev-parse HEAD)
remote_commit=$(git rev-parse "origin/$BRANCH")

if [ "$current_commit" == "$remote_commit" ]; then
    log "✅ Already up to date"
    exit 0
fi

# Pull changes
log "⬇️  Pulling changes from $BRANCH..."
git pull origin "$BRANCH"

# Install dependencies if package.json changed
if git diff --name-only "$current_commit" "$remote_commit" | grep -q "package.json"; then
    log "📦 package.json changed, installing dependencies..."
    npm install
fi

# Run build
log "🔨 Building application..."
npm run build

# Restart application
log "🔄 Restarting application..."
pm2 restart myapp

log "✅ Deployment successful!"
log "📊 Deployed commit: $(git rev-parse --short HEAD)"
log "📝 Latest commit message: $(git log -1 --pretty=%B)"
```

### Database Backup Automation

```bash
#!/bin/bash

# db-backup.sh - PostgreSQL/MongoDB backup script

set -e

# Configuration
DB_TYPE="postgres"  # or "mongodb"
DB_NAME="myapp_db"
DB_USER="dbuser"
BACKUP_DIR="/backup/database"
RETENTION_DAYS=14
DATE=$(date +%Y%m%d_%H%M%S)

mkdir -p "$BACKUP_DIR"

backup_postgres() {
    BACKUP_FILE="${BACKUP_DIR}/${DB_NAME}_${DATE}.sql.gz"
    echo "📦 Backing up PostgreSQL database: $DB_NAME"
    
    PGPASSWORD="$DB_PASSWORD" pg_dump \
        -h localhost \
        -U "$DB_USER" \
        "$DB_NAME" | gzip > "$BACKUP_FILE"
    
    echo "✅ Backup saved: $BACKUP_FILE"
}

backup_mongodb() {
    BACKUP_FILE="${BACKUP_DIR}/${DB_NAME}_${DATE}"
    echo "📦 Backing up MongoDB database: $DB_NAME"
    
    mongodump \
        --db "$DB_NAME" \
        --out "$BACKUP_FILE"
    
    tar -czf "${BACKUP_FILE}.tar.gz" -C "$BACKUP_DIR" "$(basename $BACKUP_FILE)"
    rm -rf "$BACKUP_FILE"
    
    echo "✅ Backup saved: ${BACKUP_FILE}.tar.gz"
}

# Execute backup based on type
case $DB_TYPE in
    postgres)
        backup_postgres
        ;;
    mongodb)
        backup_mongodb
        ;;
    *)
        echo "❌ Unknown database type: $DB_TYPE"
        exit 1
        ;;
esac

# Cleanup old backups
echo "🗑️  Removing backups older than $RETENTION_DAYS days"
find "$BACKUP_DIR" -type f -mtime +$RETENTION_DAYS -delete

# Show statistics
total_backups=$(find "$BACKUP_DIR" -type f | wc -l)
total_size=$(du -sh "$BACKUP_DIR" | cut -f1)
echo "📊 Total backups: $total_backups"
echo "💾 Total size: $total_size"
```

### Server Monitoring Script

```bash
#!/bin/bash

# monitor.sh - System monitoring with alerts

set -e

# Thresholds
CPU_THRESHOLD=80
MEMORY_THRESHOLD=80
DISK_THRESHOLD=85

# Alert function (email/Slack/etc.)
send_alert() {
    local subject=$1
    local message=$2
    
    echo "🚨 ALERT: $subject"
    echo "$message"
    
    # Uncomment to send email
    # echo "$message" | mail -s "$subject" admin@example.com
    
    # Uncomment to send to Slack
    # curl -X POST -H 'Content-type: application/json' \
    #   --data "{\"text\":\"$subject\n$message\"}" \
    #   YOUR_SLACK_WEBHOOK_URL
}

# Check CPU usage
cpu_usage=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d'%' -f1)
if (( $(echo "$cpu_usage > $CPU_THRESHOLD" | bc -l) )); then
    send_alert "High CPU Usage" "CPU usage is ${cpu_usage}% (threshold: ${CPU_THRESHOLD}%)"
fi

# Check memory usage
memory_usage=$(free | grep Mem | awk '{print ($3/$2) * 100.0}' | cut -d'.' -f1)
if [ "$memory_usage" -gt "$MEMORY_THRESHOLD" ]; then
    send_alert "High Memory Usage" "Memory usage is ${memory_usage}% (threshold: ${MEMORY_THRESHOLD}%)"
fi

# Check disk usage
disk_usage=$(df -h / | tail -1 | awk '{print $5}' | cut -d'%' -f1)
if [ "$disk_usage" -gt "$DISK_THRESHOLD" ]; then
    send_alert "High Disk Usage" "Disk usage is ${disk_usage}% (threshold: ${DISK_THRESHOLD}%)"
fi

# Check if services are running
services=("nginx" "postgresql" "mongodb")
for service in "${services[@]}"; do
    if ! systemctl is-active --quiet "$service"; then
        send_alert "Service Down" "$service is not running"
    fi
done

# Check application health (example: HTTP endpoint)
if ! curl -sf http://localhost:3000/health > /dev/null; then
    send_alert "Application Unhealthy" "Health check endpoint failed"
fi

echo "✅ Monitoring check completed at $(date)"
```

Add to cron (check every 5 minutes):
```bash
*/5 * * * * /path/to/monitor.sh >> /var/log/monitor.log 2>&1
```

### Process Manager (PM2) Integration

```bash
#!/bin/bash

# pm2-deploy.sh - Deploy Node.js app with PM2

set -e

APP_NAME="myapp"
APP_DIR="/var/www/myapp"
NODE_ENV="production"

cd "$APP_DIR"

# Pull latest code
git pull origin main

# Install dependencies
npm install --production

# Build if needed
if [ -f "package.json" ] && grep -q "\"build\"" package.json; then
    npm run build
fi

# PM2 operations
if pm2 describe "$APP_NAME" > /dev/null 2>&1; then
    echo "🔄 Restarting existing app..."
    pm2 restart "$APP_NAME"
else
    echo "🚀 Starting new app..."
    pm2 start npm --name "$APP_NAME" -- start
fi

# Save PM2 process list
pm2 save

# Setup PM2 to start on boot (run once)
# pm2 startup systemd -u $USER --hp $HOME

echo "✅ Deployment complete"
pm2 status
```

### Log Rotation & Management

```bash
#!/bin/bash

# rotate-logs.sh - Rotate and compress application logs

set -e

LOG_DIR="/var/www/myapp/logs"
ARCHIVE_DIR="/var/www/myapp/logs/archive"
RETENTION_DAYS=30

mkdir -p "$ARCHIVE_DIR"

# Rotate logs
for log_file in "$LOG_DIR"/*.log; do
    if [ -f "$log_file" ]; then
        filename=$(basename "$log_file")
        timestamp=$(date +%Y%m%d_%H%M%S)
        
        # Compress and move to archive
        gzip -c "$log_file" > "$ARCHIVE_DIR/${filename%.log}_${timestamp}.log.gz"
        
        # Truncate original log
        > "$log_file"
        
        echo "✅ Rotated: $filename"
    fi
done

# Remove old archives
find "$ARCHIVE_DIR" -name "*.log.gz" -mtime +$RETENTION_DAYS -delete

echo "🗑️  Cleaned up logs older than $RETENTION_DAYS days"
```

### Docker Integration

```bash
#!/bin/bash

# docker-deploy.sh - Deploy Docker containerized app

set -e

APP_NAME="myapp"
IMAGE_NAME="myapp:latest"
CONTAINER_NAME="myapp-container"

echo "🐳 Building Docker image..."
docker build -t "$IMAGE_NAME" .

# Stop and remove old container
if docker ps -a | grep -q "$CONTAINER_NAME"; then
    echo "🛑 Stopping old container..."
    docker stop "$CONTAINER_NAME"
    docker rm "$CONTAINER_NAME"
fi

# Run new container
echo "🚀 Starting new container..."
docker run -d \
    --name "$CONTAINER_NAME" \
    --restart unless-stopped \
    -p 3000:3000 \
    -e NODE_ENV=production \
    --env-file .env \
    "$IMAGE_NAME"

# Cleanup old images
echo "🧹 Cleaning up old images..."
docker image prune -f

echo "✅ Deployment complete"
docker ps | grep "$CONTAINER_NAME"
```

### SSL Certificate Management (Let's Encrypt)

```bash
#!/bin/bash

# ssl-renew.sh - Automatically renew SSL certificates

set -e

DOMAIN="example.com"
EMAIL="admin@example.com"

echo "🔒 Checking SSL certificate for $DOMAIN"

# Renew certificate
certbot renew --quiet

# Reload nginx if certificate was renewed
if [ $? -eq 0 ]; then
    echo "✅ Certificate renewed successfully"
    systemctl reload nginx
    echo "🔄 Nginx reloaded"
else
    echo "ℹ️  No renewal needed"
fi

# Check expiration
expiry_date=$(echo | openssl s_client -servername "$DOMAIN" \
    -connect "$DOMAIN":443 2>/dev/null | \
    openssl x509 -noout -dates | grep "notAfter" | cut -d= -f2)

echo "📅 Certificate expires: $expiry_date"
```

Add to cron (check daily):
```bash
0 3 * * * /path/to/ssl-renew.sh >> /var/log/ssl-renew.log 2>&1
```

---

## 🚀 Real-World Projects

### Project 1: Complete Deployment Pipeline

```bash
#!/bin/bash

# deploy-pipeline.sh - Full CI/CD deployment pipeline

set -eo pipefail

# Configuration
PROJECT="myapp"
REPO_URL="git@github.com:username/myapp.git"
BRANCH="main"
DEPLOY_DIR="/var/www/$PROJECT"
LOG_FILE="/var/log/${PROJECT}-deploy.log"

# Colors for output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

log() {
    echo -e "${GREEN}[$(date '+%Y-%m-%d %H:%M:%S')]${NC} $1" | tee -a "$LOG_FILE"
}

error() {
    echo -e "${RED}[$(date '+%Y-%m-%d %H:%M:%S')] ERROR:${NC} $1" | tee -a "$LOG_FILE"
    exit 1
}

warn() {
    echo -e "${YELLOW}[$(date '+%Y-%m-%d %H:%M:%S')] WARNING:${NC} $1" | tee -a "$LOG_FILE"
}

# Step 1: Pre-deployment checks
log "🔍 Running pre-deployment checks..."

if ! command -v git &> /dev/null; then
    error "Git is not installed"
fi

if ! command -v node &> /dev/null; then
    error "Node.js is not installed"
fi

# Step 2: Clone or update repository
log "📥 Fetching latest code..."

if [ -d "$DEPLOY_DIR" ]; then
    cd "$DEPLOY_DIR"
    git pull origin "$BRANCH" || error "Git pull failed"
else
    git clone -b "$BRANCH" "$REPO_URL" "$DEPLOY_DIR" || error "Git clone failed"
    cd "$DEPLOY_DIR"
fi

# Step 3: Backup current version
log "💾 Creating backup..."
timestamp=$(date +%Y%m%d_%H%M%S)
backup_dir="/backup/${PROJECT}_${timestamp}"
mkdir -p "$backup_dir"
tar -czf "${backup_dir}/backup.tar.gz" -C "$DEPLOY_DIR" . 2>/dev/null || warn "Backup failed"

# Step 4: Install dependencies
log "📦 Installing dependencies..."
npm ci || error "npm install failed"

# Step 5: Run tests
log "🧪 Running tests..."
if ! npm test; then
    warn "Tests failed, but continuing deployment"
fi

# Step 6: Build application
log "🔨 Building application..."
npm run build || error "Build failed"

# Step 7: Database migrations (if needed)
if [ -f "migrations.sh" ]; then
    log "🗄️  Running database migrations..."
    bash migrations.sh || error "Migration failed"
fi

# Step 8: Restart application
log "🔄 Restarting application..."

if command -v pm2 &> /dev/null; then
    pm2 restart "$PROJECT" || pm2 start npm --name "$PROJECT" -- start
    pm2 save
else
    systemctl restart "$PROJECT" || error "Service restart failed"
fi

# Step 9: Health check
log "🏥 Performing health check..."
sleep 5

health_url="http://localhost:3000/health"
if curl -sf "$health_url" > /dev/null; then
    log "✅ Health check passed"
else
    error "Health check failed - rolling back"
    # Rollback logic here
fi

# Step 10: Cleanup
log "🧹 Cleaning up..."
npm prune --production
find /backup -name "${PROJECT}_*" -mtime +7 -exec rm -rf {} \; 2>/dev/null || true

# Step 11: Post-deployment tasks
log "📊 Deployment summary:"
log "  • Commit: $(git rev-parse --short HEAD)"
log "  • Message: $(git log -1 --pretty=%B | head -n 1)"
log "  • Time: $(date)"

# Send notification
# curl -X POST -H 'Content-type: application/json' \
#   --data '{"text":"Deployment successful!"}' \
#   YOUR_SLACK_WEBHOOK

log "🎉 Deployment completed successfully!"
```

### Project 2: Server Health Dashboard Generator

```bash
#!/bin/bash

# health-dashboard.sh - Generate HTML server health dashboard

set -e

OUTPUT_FILE="/var/www/html/health.html"
REFRESH_INTERVAL=60  # seconds

generate_dashboard() {
    # Gather system info
    hostname=$(hostname)
    uptime=$(uptime -p)
    load=$(uptime | awk -F'load average:' '{print $2}')
    memory=$(free -h | awk '/^Mem:/ {print $3 "/" $2}')
    disk=$(df -h / | awk 'NR==2 {print $3 "/" $2 " (" $5 ")"}')
    cpu_temp=$(sensors 2>/dev/null | grep "Core 0" | awk '{print $3}' || echo "N/A")
    
    # Get service statuses
    services=("nginx" "postgresql" "mongodb" "redis")
    service_status=""
    for service in "${services[@]}"; do
        if systemctl is-active --quiet "$service" 2>/dev/null; then
            service_status+="<tr><td>$service</td><td style='color: green;'>✅ Running</td></tr>"
        else
            service_status+="<tr><td>$service</td><td style='color: red;'>❌ Stopped</td></tr>"
        fi
    done
    
    # Generate HTML
    cat > "$OUTPUT_FILE" << EOF
<!DOCTYPE html>
<html>
<head>
    <title>Server Health - $hostname</title>
    <meta http-equiv="refresh" content="$REFRESH_INTERVAL">
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; background: #f5f5f5; }
        .container { max-width: 800px; margin: 0 auto; background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        h1 { color: #333; border-bottom: 3px solid #4CAF50; padding-bottom: 10px; }
        .metric { margin: 15px 0; padding: 10px; background: #f9f9f9; border-left: 4px solid #4CAF50; }
        .metric label { font-weight: bold; color: #666; }
        .metric value { color: #333; float: right; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th, td { padding: 10px; text-align: left; border-bottom: 1px solid #ddd; }
        th { background: #4CAF50; color: white; }
        .timestamp { text-align: center; color: #999; margin-top: 20px; font-size: 12px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>🖥️ Server Health Dashboard</h1>
        
        <div class="metric">
            <label>Hostname:</label>
            <value>$hostname</value>
        </div>
        
        <div class="metric">
            <label>Uptime:</label>
            <value>$uptime</value>
        </div>
        
        <div class="metric">
            <label>Load Average:</label>
            <value>$load</value>
        </div>
        
        <div class="metric">
            <label>Memory Usage:</label>
            <value>$memory</value>
        </div>
        
        <div class="metric">
            <label>Disk Usage:</label>
            <value>$disk</value>
        </div>
        
        <div class="metric">
            <label>CPU Temperature:</label>
            <value>$cpu_temp</value>
        </div>
        
        <h2>📊 Services Status</h2>
        <table>
            <tr>
                <th>Service</th>
                <th>Status</th>
            </tr>
            $service_status
        </table>
        
        <div class="timestamp">
            Last updated: $(date '+%Y-%m-%d %H:%M:%S')<br>
            Auto-refresh every ${REFRESH_INTERVAL}s
        </div>
    </div>
</body>
</html>
EOF

    echo "✅ Dashboard updated: $OUTPUT_FILE"
}

# Generate dashboard
generate_dashboard

# Optional: Run continuously
# while true; do
#     generate_dashboard
#     sleep $REFRESH_INTERVAL
# done
```

Add to cron (update every minute):
```bash
* * * * * /path/to/health-dashboard.sh
```

### Project 3: Multi-Site Deployment Manager

```bash
#!/bin/bash

# multi-deploy.sh - Deploy to multiple servers

set -e

# Server configuration
declare -A SERVERS=(
    ["production"]="user@prod-server.com"
    ["staging"]="user@staging-server.com"
    ["dev"]="user@dev-server.com"
)

PROJECT_NAME="myapp"
REMOTE_PATH="/var/www/$PROJECT_NAME"

# Colors
GREEN='\033[0;32m'
RED='\033[0;31m'
NC='\033[0m'

deploy_to_server() {
    local env=$1
    local server=$2
    
    echo -e "${GREEN}🚀 Deploying to $env ($server)...${NC}"
    
    # Upload code
    rsync -avz --delete \
        --exclude 'node_modules' \
        --exclude '.git' \
        --exclude '.env' \
        ./ "$server:$REMOTE_PATH/"
    
    # Execute deployment commands on remote server
    ssh "$server" << EOF
        set -e
        cd $REMOTE_PATH
        
        echo "📦 Installing dependencies..."
        npm install --production
        
        echo "🔨 Building application..."
        npm run build
        
        echo "🔄 Restarting application..."
        pm2 restart $PROJECT_NAME || pm2 start npm --name "$PROJECT_NAME" -- start
        pm2 save
        
        echo "✅ Deployment to $env complete!"
EOF
    
    if [ $? -eq 0 ]; then
        echo -e "${GREEN}✅ Successfully deployed to $env${NC}"
    else
        echo -e "${RED}❌ Deployment to $env failed${NC}"
        return 1
    fi
}

# Main deployment logic
main() {
    if [ $# -eq 0 ]; then
        echo "Usage: $0 <environment>"
        echo "Available environments: ${!SERVERS[@]}"
        exit 1
    fi
    
    env=$1
    
    if [ -z "${SERVERS[$env]}" ]; then
        echo "❌ Unknown environment: $env"
        echo "Available: ${!SERVERS[@]}"
        exit 1
    fi
    
    read -p "Deploy to $env? (y/N) " -n 1 -r
    echo
    if [[ ! $REPLY =~ ^[Yy]$ ]]; then
        echo "Deployment cancelled"
        exit 0
    fi
    
    deploy_to_server "$env" "${SERVERS[$env]}"
}

main "$@"
```

---

## 📚 Best Resources

### 📖 Books (Free & Paid)
- **The Linux Command Line** by William Shotts (Free PDF)
- **Linux Bible** by Christopher Negus
- **Shell Scripting: Expert Recipes for Linux, Bash** by Steve Parker
- **UNIX and Linux System Administration Handbook** by Evi Nemeth

### 🎥 Video Courses
- **freeCodeCamp** - Linux for Beginners (YouTube)
- **Traversy Media** - Linux Crash Course
- **Net Ninja** - Linux Terminal Tutorial
- **Corey Schafer** - Linux/Mac Terminal Tutorial

### 🌐 Interactive Learning
- **OverTheWire: Bandit** (wargames)
