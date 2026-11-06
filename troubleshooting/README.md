# Linux & Application Troubleshooting

## Overview

This section documents practical troubleshooting scenarios performed on an Ubuntu EC2 instance running a Node.js application behind NGINX.

The exercises involved investigating service failures, systemd errors, HTTP 502 responses, application logs, and CPU and memory usage. Each scenario focused on identifying the problem, investigating relevant system information, applying corrective actions, and verifying recovery.

## Environment

| Component | Details |
|---|---|
| OS | Ubuntu 24.04 LTS |
| Infrastructure | AWS EC2 |
| Application | Node.js |
| Reverse Proxy | NGINX |
| Service Management | systemd |
| Logging | journalctl, NGINX logs |

---

## 1. Troubleshooting systemd Service Failures

### Investigating a working-directory error

Encountered a systemd service startup failure related to the application's working directory (`chdir`).

The error was investigated using service status and logs to understand why systemd could not start the application correctly.

![systemd chdir error](./screenshots/01-systemd-chdir-error.png)

### Investigation commands

```bash
sudo systemctl status nodejs
sudo journalctl -u nodejs
sudo journalctl -u nodejs -n 50
```

The investigation involved checking the service configuration and its working-directory settings.

### Service recovery

After addressing the service configuration issue, the application service was recovered and its status verified.

![Service recovery](./screenshots/03-service-recovery.png)

---

## 2. Investigating Service Logs and Permission Errors

Linux service logs are an important source of information when diagnosing application failures.

During the exercise, a permission-related error was encountered while accessing logs.

![journalctl permission denied](./screenshots/02-journalctl-permission-denied.png)

### Commands for log investigation

```bash
# Inspect service status
sudo systemctl status nodejs

# View service logs
sudo journalctl -u nodejs

# View recent logs
sudo journalctl -u nodejs -n 50

# Follow logs in real time
sudo journalctl -u nodejs -f
```

This exercise involved investigating service logs and understanding the permissions required to access system-level information.

---

## 3. Troubleshooting NGINX 502 Bad Gateway

### Problem

The Node.js application was accessed through NGINX as a reverse proxy. During testing, an HTTP **502 Bad Gateway** response was encountered.

![NGINX 502 Bad Gateway](./screenshots/linux-nginx-nodejs-502-error.png)

A 502 response indicates that NGINX was unable to obtain a valid response from its upstream application.

### Investigation

The troubleshooting process involved examining the application and proxy components:

- Checking whether the Node.js service was running.
- Inspecting systemd service status.
- Reviewing NGINX error logs.
- Checking the application's listening address.
- Testing the application through NGINX.

### NGINX error logs

![NGINX error logs](./screenshots/linux-nginx-nodejs-error-logs.png)

### Useful commands

```bash
# Check Node.js service
sudo systemctl status nodejs

# Check NGINX service
sudo systemctl status nginx

# Inspect NGINX error logs
sudo tail -n 50 /var/log/nginx/error.log

# Check listening ports
sudo ss -tulpn

# Test application through NGINX
curl -i http://localhost/
```

### Recovery and validation

After investigating the application and NGINX configuration, the services were recovered and the application was tested again.

![NGINX and Node.js service recovery](./screenshots/linux-nginx-nodejs-service-recovery.png)

---

## 4. Investigating Node.js Listening Addresses

The Node.js application was tested with different listening configurations to understand the difference between binding to localhost and binding to all network interfaces.

### Localhost-only binding

![Node.js listening on localhost](./screenshots/nodejs-listening-localhost-only.png)

### Listening on all interfaces

![Node.js listening on all interfaces](./screenshots/nodejs-listening-all-interfaces.png)

### Verification

The listening address and port were inspected using:

```bash
sudo ss -tulpn
```

This helped demonstrate how the application's binding address affects where it can accept connections.

The application was also accessed through NGINX:

![Node.js running through NGINX](./screenshots/nodejs-running-through-nginx.png)

---

## 5. Troubleshooting Stopped Services

Service failures were deliberately examined by checking the service state and reviewing logs.

### Node.js service stopped

![Node.js service stopped](./screenshots/nodejs-systemd-stopped.png)

### Inspecting Node.js logs

![Node.js journal logs](./screenshots/nodejs-journelctl.png)

### NGINX service stopped

![NGINX service stopped](./screenshots/nginx-systemd-stopped.png)

The exercises reinforced the importance of checking both the application service and the reverse proxy when investigating application availability.

---

## 6. CPU Resource Investigation

CPU stress testing was performed to observe resource utilization and system behavior under increased load.

![CPU saturation](./screenshots/01-cpu-saturation.png)

### Useful commands

```bash
# View CPU and process activity
top

# Inspect load average
uptime

# Find CPU-intensive processes
ps aux --sort=-%cpu | head
```

This exercise helped connect Linux resource monitoring with the EC2 Auto Scaling work documented separately.

---

## 7. Memory Usage Investigation and Recovery

A controlled memory allocation test was used to investigate process-level memory usage.

### Baseline

System memory usage was inspected before introducing the test workload.

![Memory baseline](./screenshots/memory-baseline.png)

### Detecting increased memory usage

A controlled process allocated approximately 200 MiB of memory. Memory usage and process activity were inspected during the test.

![Memory process detected](./screenshots/memory-process-detected.png)

### Process investigation

The process was investigated using Linux memory and process information.

![Memory process investigation](./screenshots/memory-process-investigation.png)

Commands used during the investigation included:

```bash
# Inspect memory usage
free -h

# Identify memory-intensive processes
ps aux --sort=-%mem | head

# Inspect process details
cat /proc/<PID>/status
```

### Recovery

The test process was terminated, and memory usage was checked again to confirm recovery.

![Memory recovery](./screenshots/memory-recovery.png)

This was a controlled process-level memory investigation. It was not a system-wide out-of-memory event.

---

## Key Skills Demonstrated

- Linux service troubleshooting using systemd
- Application log investigation using journalctl
- NGINX reverse proxy troubleshooting
- HTTP 502 investigation
- Process and listening-port inspection
- CPU and memory utilization investigation
- Controlled resource testing and recovery
- Service health verification after troubleshooting

