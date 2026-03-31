# InfraWatch

InfraWatch is a centralized server management platform designed to help administrators monitor and manage multiple Linux and Windows servers from a single web interface. The system provides real-time health monitoring, remote configuration capabilities, automated alerts, and centralized logging. InfraWatch simplifies the complex task of managing distributed server infrastructure by offering an intuitive dashboard where you can view system metrics, execute commands, manage network settings, and receive notifications about potential issues before they become critical problems.

---

## Table of Contents

1. [Installation - Docker Method (Recommended)](#installation---docker-method-recommended)
2. [Installation - Manual Method](#installation---manual-method)
3. [Server Controller Setup](#server-controller-setup)
4. [First Time Access](#first-time-access)
5. [Troubleshooting](#troubleshooting)

---

## Installation - Docker Method (Recommended)

The Docker-based installation is the easiest way to get InfraWatch running. It automatically sets up all required components including the database, backend server, and frontend interface.

### Prerequisites

Before starting, you need to install the following software on your computer:

**For Windows Users:**

1. **Git for Windows** - Download from https://git-scm.com/download/win
2. **Docker Desktop** - Download from https://www.docker.com/products/docker-desktop
3. **Windows Subsystem for Linux (WSL2)** - Open PowerShell as Administrator and run:
   ```powershell
   wsl --install
   ```
4. **Enable Virtualization** - This must be enabled in your computer's BIOS settings. Restart your computer, press the BIOS key (usually F2, F10, or DEL during startup), and enable "Intel VT-x" or "AMD-V" depending on your processor.

**For Linux Users:**

1. **Git** - Install using your package manager:
   ```bash
   # For Ubuntu/Debian
   sudo apt update
   sudo apt install git
   
   # For Fedora/RHEL
   sudo dnf install git
   
   # For Arch Linux
   sudo pacman -S git
   ```

2. **Docker and Docker Compose** - Install using:
   ```bash
   # For Ubuntu/Debian
   sudo apt update
   sudo apt install docker.io docker-compose
   sudo systemctl start docker
   sudo systemctl enable docker
   sudo usermod -aG docker $USER
   
   # For Fedora/RHEL
   sudo dnf install docker docker-compose
   sudo systemctl start docker
   sudo systemctl enable docker
   sudo usermod -aG docker $USER
   
   # For Arch Linux
   sudo pacman -S docker docker-compose
   sudo systemctl start docker
   sudo systemctl enable docker
   sudo usermod -aG docker $USER
   ```
   
   **Important:** After running these commands, log out and log back in for the group changes to take effect.

### Step 1: Download the Project

You need to download the InfraWatch project files to your computer. There are two methods:

**Method A: Using Git (Recommended)**

Open a terminal (Linux) or Command Prompt/PowerShell (Windows) and run:

```bash
git clone https://github.com/K1sh0rx/InfraWatch.git
```

This command downloads the entire project into a folder called "Inf.raWatch" in your current directory.

**Method B: Download as ZIP**

1. Go to https://github.com/K1sh0rx/InfraWatch
2. Click the green "Code" button
3. Select "Download ZIP"
4. Extract the ZIP file to a location of your choice

### Step 2: Navigate to the Project Directory

After downloading, you need to open a terminal inside the project folder.

**For Linux:**
```bash
cd InfraWatch
```

**For Windows (PowerShell):**
```powershell
cd InfraWatch
```

**For Windows (Command Prompt):**
```cmd
cd InfraWatch
```

To verify you are in the correct directory, list the files:

**For Linux:**
```bash
ls
```

**For Windows (PowerShell):**
```powershell
dir
```

You should see output similar to this:
```
backend/
client/
db/
docker-compose.yml
docs/
frontend/
README.md
setup.ps1
setup.sh
```

If you see these folders and files, you are in the correct location.

### Step 3: Run the Setup Script

The setup script will guide you through the installation process. It will ask you a few questions and then automatically configure everything.

**For Linux:**

First, make the script executable:
```bash
chmod +x setup.sh
```

Then run it:
```bash
./setup.sh start
```

**For Windows (PowerShell):**

Run PowerShell as Administrator (right-click PowerShell and select "Run as Administrator"), then:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
.\setup.ps1 start
```

### Step 4: Answer the Setup Questions

The script will ask you several questions. Here's what each means and how to answer:

**Question 1: Select Host IP Address**

The script will show you a list of IP addresses available on your computer:
```
Available host IP addresses:
  1) 192.168.1.100   (default/active)
  2) 10.0.0.50
Select host IP [1-2] (default: 1):
```

**What this means:** The IP address is how other computers on your network will access InfraWatch. If you're just testing on your own computer, you can press Enter to accept the default. If you want other computers to access it, choose the IP address that's on your local network (usually starts with 192.168).

**How to answer:** Press Enter to use the default, or type the number corresponding to your preferred IP and press Enter.

**Question 2: Enter DATABASE Port**

```
Enter DATABASE port (default: 9001):
```

**What this means:** This is the network port where the PostgreSQL database will listen for connections. The database stores all your server information, metrics, and logs.

**How to answer:** Press Enter to use the default port 9001, or type a different port number if 9001 is already in use by another program on your computer.

**Question 3: Enter BACKEND Port**

```
Enter BACKEND port (default: 9000):
```

**What this means:** This is the port where the InfraWatch backend API server will run. The frontend communicates with the backend through this port.

**How to answer:** Press Enter to use the default port 9000, or type a different port number if needed.

### Step 5: Wait for Installation to Complete

After answering the questions, the script will:

1. Generate configuration files (.env files) with your settings
2. Download and build Docker images (this may take 5-10 minutes the first time)
3. Start the database, backend, and frontend containers
4. Initialize the database with the required tables and structure

You will see various messages scrolling on the screen. This is normal. Wait until you see:

```
System setup complete!
Frontend:    http://192.168.1.100
Backend API: http://192.168.1.100:9000
PostgreSQL:  Port 9001
```

The IP addresses and ports shown will match what you selected during setup.

### Step 6: Access InfraWatch

Open your web browser and go to the Frontend URL shown in the completion message. For example:

```
http://192.168.1.100
```

You should see the InfraWatch login page.

**Default Login Credentials:**
- Username: `admin`
- Password: `admin`

**Important:** Change the default password immediately after your first login for security reasons.

---

## Installation - Manual Method

The manual installation method gives you more control over the setup process but requires more technical knowledge and manual configuration. Use this method if you need to customize the installation or if you prefer not to use Docker.

### Prerequisites

Before starting manual installation, ensure you have the following software installed:

1. **PostgreSQL Database (version 15 or higher)**
   - Download from https://www.postgresql.org/download/
   
2. **Go Programming Language (version 1.20 or higher)**
   - Download from https://go.dev/dl/
   
3. **Node.js JavaScript Runtime (version 18 or higher)**
   - Download from https://nodejs.org/
   
4. **sqlc Code Generator**
   - Install using:
   ```bash
   go install github.com/sqlc-dev/sqlc/cmd/sqlc@latest
   ```
   
5. **Git Version Control**
   - Download from https://git-scm.com/

### Step 1: Download the Project

Clone the repository:
```bash
git clone https://github.com/K1sh0rx/Inf.raWatch.git
cd InfraWatch
```

### Step 2: Set Up PostgreSQL Database

Start the PostgreSQL service:

**For Linux:**
```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

**For Windows:**
PostgreSQL should start automatically after installation. If not, search for "Services" in the Start menu, find "postgresql-x64-15", and click "Start".

Create a database user and database:

```bash
# Open PostgreSQL command line
# For Linux:
sudo -u postgres psql

# For Windows:
# Search for "SQL Shell (psql)" in the Start menu and open it
# Press Enter for default values, then enter the password you set during PostgreSQL installation
```

Once in the PostgreSQL prompt, run these commands:

```sql
CREATE USER admin WITH PASSWORD 'admin';
CREATE DATABASE smsdb;
GRANT ALL PRIVILEGES ON DATABASE smsdb TO admin;
\c smsdb
GRANT ALL ON SCHEMA public TO admin;
```

To exit PostgreSQL:
```sql
\q
```

Test the connection:
```bash
psql -U admin -d smsdb -h localhost -p 5432
```

When prompted, enter the password: `admin`

If you successfully connect, type `\q` to exit.

### Step 3: Configure the Backend

Navigate to the backend directory:
```bash
cd backend
```

Generate database access code using sqlc:
```bash
cd db
sqlc generate
cd ..
```

Create a file named `.env` in the `backend` directory with the following content:

```
DATABASE_URL=postgres://admin:admin@localhost:5432/smsdb?sslmode=disable
CLIENT_PORT=2210
CLIENT_PROTOCOL=http
JWT_SECRET=xxxxxxxxxxxxxxxx
SERVER_PORT=8000
LOG_LEVEL=info
SMTP_HOST=smtp.google.com
SMTP_PORT=574
SMTP_USERNAME=xxxxxxxxxxxxx@gmail.com
SMTP_PASSWORD=xxxxxxxxxxxxxx
SMTP_FROM=SMS Alerts <xxxxxxxxxxxxx@gmail.com>
```

**Note:** Replace the database connection details if you used different values during PostgreSQL setup.

Initialize the database structure:
```bash
go run temp/dbinit.go
```

You should see messages indicating that tables were created successfully.

Build the backend server:
```bash
go build -o server main.go
```

This creates an executable file named `server` (or `server.exe` on Windows).

### Step 4: Configure the Frontend

Open a new terminal window and navigate to the frontend directory:
```bash
cd frontend
```

Install the required Node.js packages:
```bash
npm install
```

This will download all dependencies. It may take a few minutes.

Create a file named `.env` in the `frontend` directory:

```
VITE_BACKEND_URL=http://localhost:9000
```

**Note:** Replace `localhost` with your computer's IP address if you want to access InfraWatch from other computers on your network.

Build the frontend:
```bash
npm run build
```

### Step 5: Start the Services

You need to start both the backend and frontend in separate terminal windows.

**Terminal 1 - Start Backend:**
```bash
cd backend
./server
```

On Windows:
```cmd
cd backend
server.exe
```

You should see output like:
```
Server running on port 8000
Database connected successfully
```

**Terminal 2 - Start Frontend:**
```bash
cd frontend
npm run preview
```

You should see output like:
```
Local:   http://localhost:4173/
Network: http://192.168.1.100:4173/
```

### Step 6: Access InfraWatch

Open your web browser and navigate to the URL shown in the frontend terminal (usually http://localhost:4173).

**Default Login Credentials:**
- Username: `admin`
- Password: `admin`

**Important:** Change the default password immediately after your first login.

---

## Server Controller Setup

The Server Controller is a lightweight program that runs on each server you want to manage with InfraWatch. It communicates with the central backend to send health metrics and receive commands.

### Prerequisites

The server you want to monitor must have:
- Go programming language (version 1.20 or higher)
- Git
- Network connectivity to the InfraWatch backend server

### Step 1: Download the Project on the Target Server

On the server you want to monitor, run:

```bash
git clone https://github.com/K1sh0rx/InfraWatch.git
cd Inf.raWatch
```

### Step 2: Navigate to the Correct Client Directory

**For Linux Servers:**
```bash
cd client/linux
```

**For Windows Servers:**
```cmd
cd client\windows
```

### Step 3: Install Dependencies

```bash
go mod tidy
```

This command downloads all required Go packages for the controller.

### Step 4: Build the Controller

**For Linux:**
```bash
go build -o controller main.go
```

**For Windows:**
```cmd
go build -o controller.exe main.go
```

### Step 5: Generate an Access Token

Before running the controller, you need to generate an access token from the InfraWatch admin panel:

1. Log in to the InfraWatch web interface
2. Navigate to "Configuration" or "Settings"
3. Click on "Server Management" or "Add New Server"
4. Click "Generate Token" or "Create Access Token"
5. Copy the generated token (it will look like a long random string)

### Step 6: Run the Controller

**For Linux:**
```bash
./controller
```

**For Windows:**
```cmd
controller.exe
```

On first run, the controller will prompt you to enter the access token:
```
Enter access token:
```

Paste the token you copied from the admin panel and press Enter.

The controller will save this token and use it for all future connections. You should see output like:
```
Controller started successfully
Connected to backend: http://192.168.1.100:9000
Sending health metrics every 30 seconds
```

### Step 7: Verify Connection

Return to the InfraWatch web interface and check the server list. You should see your newly connected server appear with its hostname, IP address, and current status.

### Running Controller as a Background Service

To keep the controller running even after you close the terminal:

**For Linux (using systemd):**

Create a service file:
```bash
sudo nano /etc/systemd/system/infrawatch-controller.service
```

Add the following content (adjust paths as needed):
```ini
[Unit]
Description=InfraWatch Server Controller
After=network.target

[Service]
Type=simple
User=your-username
WorkingDirectory=/path/to/Inf.raWatch/client/linux
ExecStart=/path/to/Inf.raWatch/client/linux/controller
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable and start the service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable infrawatch-controller
sudo systemctl start infrawatch-controller
```

Check status:
```bash
sudo systemctl status infrawatch-controller
```

**For Windows (using Task Scheduler):**

1. Open Task Scheduler (search for it in the Start menu)
2. Click "Create Basic Task"
3. Name it "InfraWatch Controller"
4. Set trigger to "When the computer starts"
5. Set action to "Start a program"
6. Browse to the location of `controller.exe`
7. Finish the wizard
8. Right-click the task and select "Run" to start it immediately

### Resetting the Access Token

If you need to change the access token:

**For Linux:**
```bash
rm auth/token.hash
./controller
```

**For Windows:**
```cmd
del auth\token.hash
controller.exe
```

The controller will ask for a new token on the next run.

---

## First Time Access

After successful installation, follow these steps to get started with InfraWatch:

### Step 1: Initial Login

1. Open your web browser
2. Navigate to the InfraWatch URL (e.g., http://192.168.1.100)
3. You will see the login page
4. Enter the default credentials:
   - Username: `admin`
   - Password: `admin`
5. Click "Login"

### Step 2: Change Default Password

For security reasons, you should immediately change the default password:

1. After logging in, look for "Settings" in the navigation menu
2. Click on "User Management" or "Account Settings"
3. Find the "Change Password" option
4. Enter a strong new password
5. Confirm the new password
6. Click "Save" or "Update Password"

### Step 3: Add Your First Server

1. Navigate to "Configuration" or "Servers" in the menu
2. Click "Add New Server" or "Register Device"
3. Fill in the server details:
   - **Server Name:** A friendly name for identification (e.g., "Web Server 1")
   - **IP Address:** The IP address of the server you want to monitor
   - **Operating System:** Select Linux or Windows
4. Click "Generate Access Token"
5. Copy the generated token
6. Follow the [Server Controller Setup](#server-controller-setup) instructions to install the controller on that server
7. When prompted, paste the access token

### Step 4: Explore the Dashboard

After adding a server, explore the different sections:

- **Health Dashboard:** View real-time CPU, memory, disk, and network metrics
- **Alerts:** See any warnings or critical issues detected on your servers
- **Logs:** Browse system logs from all connected servers
- **Configuration:** Manage server settings, network configuration, and firewall rules
- **Resource Optimization:** View and manage running services, perform cleanup operations

---

## Troubleshooting

### Docker Installation Issues

**Problem: "docker: command not found"**

Solution: Docker is not installed or not in your system PATH.
- Verify Docker installation: `docker --version`
- Reinstall Docker if necessary
- On Linux, ensure Docker service is running: `sudo systemctl start docker`

**Problem: "permission denied while trying to connect to the Docker daemon"**

Solution: Your user does not have permission to use Docker.
- On Linux: `sudo usermod -aG docker $USER`
- Log out and log back in
- Verify with: `groups` (you should see "docker" in the list)

**Problem: Setup script fails with "port already in use"**

Solution: Another program is using the required port.
- Check what's using the port: `sudo lsof -i :9000` (replace 9000 with your port)
- Either stop that program or run the setup script again and choose a different port

**Problem: Database initialization failed**

Solution: The database container may not be ready yet.
- Wait 30 seconds and run: `docker exec sms-backend ./dbinit`
- If it still fails, check logs: `docker logs sms-backend`

### Manual Installation Issues

**Problem: "psql: error: connection to server failed"**

Solution: PostgreSQL is not running or connection details are incorrect.
- Check if PostgreSQL is running:
  - Linux: `sudo systemctl status postgresql`
  - Windows: Check Services panel
- Verify port 5432 is open: `netstat -an | grep 5432`
- Check connection string in backend/.env matches your PostgreSQL setup

**Problem: "sqlc: command not found"**

Solution: sqlc is not installed or not in PATH.
- Install sqlc: `go install github.com/sqlc-dev/sqlc/cmd/sqlc@latest`
- Ensure Go bin directory is in PATH: `export PATH=$PATH:$(go env GOPATH)/bin`
- Verify installation: `sqlc version`

**Problem: npm install fails with "EACCES" error**

Solution: Permission issues with npm global directory.
- Fix npm permissions: https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally
- Or use a Node version manager like nvm

**Problem: Frontend shows "Cannot connect to backend"**

Solution: Backend is not running or frontend is configured with wrong URL.
- Check if backend is running: Look for the terminal where you started it
- Verify backend URL in frontend/.env matches where backend is running
- Check if firewall is blocking the connection

### Controller Issues

**Problem: "Connection refused" when starting controller**

Solution: Backend server is not accessible.
- Verify backend is running and accessible
- Check if the IP address and port are correct
- Test connection: `curl http://YOUR_BACKEND_IP:9000/health` (Linux/Mac) or use browser
- Check firewall rules on both controller and backend machines

**Problem: Token authentication failed**

Solution: Token is invalid or expired.
- Delete the token file and regenerate:
  - Linux: `rm auth/token.hash`
  - Windows: `del auth\token.hash`
- Generate a new token in the admin panel
- Restart the controller and enter the new token

**Problem: Controller stops unexpectedly**

Solution: Various possible causes.
- Check logs in the controller terminal
- Ensure stable network connection to backend
- Verify sufficient system resources (CPU, memory)
- Consider running as a system service for auto-restart

### General Issues

**Problem: Cannot access InfraWatch from another computer**

Solution: Firewall or network configuration blocking access.
- Ensure the computer running InfraWatch has firewall rules allowing incoming connections on the required ports
- Verify both computers are on the same network
- Try accessing using the IP address instead of localhost

**Problem: Login page loads but credentials don't work**

Solution: Database may not be initialized properly.
- Check if database containers/service is running
- Re-run database initialization:
  - Docker: `docker exec sms-backend ./dbinit`
  - Manual: `cd backend && go run temp/dbinit.go`

**Problem: Dashboard is slow or unresponsive**

Solution: Browser cache or resource constraints.
- Clear browser cache and cookies
- Try a different browser
- Check if backend server has sufficient resources
- Look for errors in browser console (F12, then Console tab)

### Getting Help

If you encounter issues not covered here:

1. Check the project repository issues: https://github.com/K1sh0rx/Inf.raWatch/issues
2. Search for similar problems others may have reported
3. Create a new issue with:
   - Detailed description of the problem
   - Steps to reproduce
   - Error messages (if any)
   - Your operating system and versions
   - Installation method used (Docker or Manual)

---

## Maintenance Commands

### Docker Installation

**Stop all containers:**
```bash
docker compose stop
```

**Start stopped containers:**
```bash
docker compose start
```

**Restart containers:**
```bash
docker compose restart
```

**View container logs:**
```bash
docker logs sms-backend
docker logs sms-frontend
docker logs sms-db
```

**Clean up everything and start fresh:**
```bash
./setup.sh clean
./setup.sh start
```

**Update to latest version:**
```bash
git pull
docker compose down
docker compose up -d --build
```

### Manual Installation

**Restart backend:**
- Stop the backend terminal (Ctrl+C)
- Navigate to backend directory
- Run `./server` (or `server.exe` on Windows)

**Restart frontend:**
- Stop the frontend terminal (Ctrl+C)
- Navigate to frontend directory
- Run `npm run preview`

**Update to latest version:**
```bash
git pull
cd backend && go build -o server main.go
cd ../frontend && npm install && npm run build
```

**Backup database:**
```bash
pg_dump -U admin -d smsdb > infrawatch_backup.sql
```

**Restore database:**
```bash
psql -U admin -d smsdb < infrawatch_backup.sql
```

---

## Support and Documentation

For additional help and documentation:

- Project Repository: https://github.com/K1sh0rx/InfraWatch
- Issues and Bug Reports: https://github.com/K1sh0rx/InfraWatch/issues
- Full Documentation: See the `docs` folder in the project directory

---

## License

Please refer to the LICENSE file in the project repository for licensing information.


##  Maintainers / Contact

```
Maintained by:
- Kishore [@kishore-001](https://github.com/kishore-001)
- Karthick Raghul S J [@KarthickRaghul](https://github.com/KarthickRaghul)
- Thiruvel [@Thiruvelhere](https://github.com/Thiruvelhere)
- Harjeet 
```
