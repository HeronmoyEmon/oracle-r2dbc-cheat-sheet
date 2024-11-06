# oracle-r2dbc-cheat-sheet

To set up Oracle Database 19c on your Linux machine, follow these steps. Oracle 19c is available for Oracle Linux and Red Hat-based distributions (e.g., CentOS, Fedora) and can also be configured on Ubuntu. Here's a complete setup guide:

## 1. Download Oracle Database 19c
1. Go to the [Oracle Database 19c download page](https://www.oracle.com/database/technologies/oracle19c-linux-downloads.html).
2. Download the Oracle Database 19c installation files, specifically the `LINUX.X64_193000_db_home.zip` file.

## 2. Install Required Packages and Dependencies
Oracle Database requires certain packages. Install these with the following commands (adjust for your package manager):
For **Oracle Linux**, **RHEL**, or **CentOS**:
```bash
sudo yum install -y oracle-database-preinstall-19c
```

For **Ubuntu**/**Debian**:
```bash
sudo apt update
sudo apt install -y alien libaio1 bc flex
```

## 3. Prepare the Oracle User and Groups
Oracle recommends creating a dedicated user for running the database.
1. **Create a new user and group** for Oracle:
```bash
sudo groupadd oinstall
sudo groupadd dba
sudo useradd -m -g oinstall -G dba oracle
```

2. **Set a password** for the `oracle` user:
```bash
sudo passwd oracle
```

3. **Set up directories** for Oracle installation and data:
```bash
sudo mkdir -p /opt/oracle/product/19c/dbhome_1
sudo mkdir -p /opt/oracle/oradata
sudo chown -R oracle:oinstall /opt/oracle
sudo chmod -R 775 /opt/oracle
```

## 4. Configure Kernel Parameters
Edit the kernel parameters file to optimize for Oracle Database (for CentOS/RHEL-based systems). Open `/etc/sysctl.conf` and add:
```conf
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax = 1073741824  # Adjust based on your RAM, e.g., 50% of total RAM
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
```
Apply the changes:
```bash
sudo sysctl -p
```

## 5. Extract and Install Oracle Database 19c
1. Switch to the Oracle user:
```bash
su - oracle
```

2. Extract the downloaded Oracle Database 19c installation files:
```bash
unzip /path/to/LINUX.X64_193000_db_home.zip -d /opt/oracle/product/19c/dbhome_1
```

3. Run the `Oracle` Installer:
```bash
cd /opt/oracle/product/19c/dbhome_1
./runInstaller
```
During installation, you’ll be prompted to:

- Select Oracle Base (e.g., /opt/oracle).
- Select Oracle Home (/opt/oracle/product/19c/dbhome_1).
- Choose configuration options and set up a new database.

Follow the prompts to complete the installation.
