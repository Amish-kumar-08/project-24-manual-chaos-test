# Project 24: Manual Chaos Test

## Description

Performed a manual chaos engineering experiment on an AWS EC2 instance by intentionally stopping the Nginx web server and verifying service failure. Restarted the service and confirmed successful recovery using systemctl and HTTP response checks.

The project demonstrates basic failure injection, service monitoring, incident verification, and manual recovery using AWS Systems Manager (SSM).

## Objectives

- Understand manual chaos testing.
- Connect to an EC2 instance using AWS Systems Manager.
- Install and configure the Nginx web server.
- Simulate service failure by stopping Nginx.
- Verify service unavailability.
- Recover the service by restarting Nginx.
- Validate successful recovery using HTTP responses.

## AWS Resources Used

| Resource | Configuration |
|---|---|
| Amazon EC2 | t2.micro |
| Operating System | Ubuntu 24.04.5 LTS |
| AWS Region | ap-south-1 (Mumbai) |
| AWS Systems Manager | Session Manager |
| Web Server | Nginx 1.24.0 |

## Technologies

- AWS EC2
- AWS Systems Manager (SSM)
- Ubuntu Linux
- Nginx
- Linux systemctl
- Linux command line
- cURL

## Implementation Steps

### Step 1: Connect to EC2 using SSM

Connected to the existing Ubuntu EC2 instance using AWS Systems Manager.

```bash
aws ssm start-session \
  --target i-02b6a5d0ac01f3ae3 \
  --region ap-south-1
```

### Step 2: Install Nginx

Updated the package repositories and installed Nginx.

```bash
sudo apt update
sudo apt install nginx -y
```

Verified the installation:

```bash
nginx -v
```

### Step 3: Start and Verify Nginx

Started the Nginx service and enabled it at system startup.

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

Checked service status:

```bash
sudo systemctl status nginx
```

Verified HTTP response:

```bash
curl -I http://localhost
```

Expected result:

```text
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
```

### Step 4: Inject Manual Failure

Intentionally stopped the Nginx service.

```bash
sudo systemctl stop nginx
```

Verified service status:

```bash
sudo systemctl is-active nginx
```

Result:

```text
inactive
```

Tested HTTP availability:

```bash
curl -I --max-time 5 http://localhost
```

Result:

```text
curl: (7) Failed to connect to localhost port 80
```

This confirmed that the web server became unavailable after failure injection.

### Step 5: Recover the Service

Restarted Nginx:

```bash
sudo systemctl start nginx
```

Verified the service:

```bash
sudo systemctl status nginx
```

Result:

```text
Active: active (running)
```

Performed the final HTTP test:

```bash
curl -I http://localhost
```

Result:

```text
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
```

The successful HTTP response confirmed that the service had recovered.

## Test Results

| Test | Expected Result | Actual Result |
|---|---|---|
| Initial service status | Running | Running |
| Initial HTTP request | 200 OK | 200 OK |
| Stop Nginx | Service stopped | Successful |
| Service status after failure | Inactive | Inactive |
| HTTP request after failure | Connection refused | Connection refused |
| Restart Nginx | Service running | Successful |
| Final HTTP request | 200 OK | 200 OK |

## Screenshots

Add your project screenshots in the `screenshots/` directory.

1. Normal Nginx service operation.
2. Manual failure injection and failed HTTP request.
3. Successful service recovery and HTTP 200 response.

## Learning Outcomes

- Learned how to perform manual chaos testing.
- Understood service failure and recovery.
- Practiced Linux service management using systemctl.
- Used AWS Systems Manager for remote EC2 access.
- Verified application availability using cURL.
- Understood the importance of incident documentation and recovery verification.

## Conclusion

Successfully completed a manual chaos testing experiment on an AWS EC2 instance.

The Nginx service was intentionally stopped to simulate a failure. Service unavailability was verified through systemctl and cURL. The service was subsequently restarted, and successful recovery was confirmed through an HTTP 200 OK response.

**Project Status: Completed**
