# Incident Report: Project 24 – Manual Chaos Test

## 1. Incident Information

| Field | Details |
|---|---|
| Project | Project 24: Manual Chaos Test |
| Incident Type | Manual Service Failure |
| Service | Nginx Web Server |
| Cloud Platform | AWS |
| AWS Region | ap-south-1 (Mumbai) |
| EC2 Instance | i-02b6a5d0ac01f3ae3 |
| Operating System | Ubuntu 24.04.5 LTS |
| Incident Date | 24 November 2026 |
| Status | Recovered |

## 2. Incident Objective

The objective was to simulate a service failure on an AWS EC2 instance by manually stopping the Nginx web server.

The experiment aimed to verify service unavailability, perform manual recovery, and confirm that the application returned to normal operation.

## 3. Initial System State

Before injecting failure, Nginx was installed and running successfully.

**Verification command:**

```bash
sudo systemctl status nginx
```

**Result:**

```text
Active: active (running)
```

HTTP availability was verified using:

```bash
curl -I http://localhost
```

**Result:**

```text
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
```

The service was operating normally before the experiment.

## 4. Failure Injection

A manual failure was introduced by stopping the Nginx service.

**Command executed:**

```bash
sudo systemctl stop nginx
```

The service state was checked using:

```bash
sudo systemctl is-active nginx
```

**Result:**

```text
inactive
```

## 5. Incident Impact

After stopping Nginx, HTTP requests to the local web server failed.

**Verification command:**

```bash
curl -I --max-time 5 http://localhost
```

**Observed result:**

```text
curl: (7) Failed to connect to localhost port 80
```

**Impact:**

- Nginx stopped serving HTTP requests.
- The local web server became unavailable.
- HTTP connection attempts were refused.

The impact was limited to the test performed on the selected EC2 instance.

## 6. Recovery Procedure

The Nginx service was manually restarted.

**Recovery command:**

```bash
sudo systemctl start nginx
```

The service status was verified:

```bash
sudo systemctl status nginx
```

**Result:**

```text
Active: active (running)
```

## 7. Recovery Verification

After restarting Nginx, an HTTP request was performed.

**Command:**

```bash
curl -I http://localhost
```

**Observed result:**

```text
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
```

The service successfully returned an HTTP 200 response, confirming recovery.

## 8. Root Cause

The service interruption was intentionally introduced as part of the manual chaos testing exercise.

The immediate cause of the service unavailability was the execution of:

```bash
sudo systemctl stop nginx
```

No unexpected application or infrastructure failure was involved in the experiment.

## 9. Lessons Learned

- A running service can become unavailable when its process is stopped.
- systemctl helps monitor and manage Linux services.
- cURL provides a simple way to verify HTTP availability.
- Manual recovery can restore service availability.
- Incident documentation is important for recording failures and recovery actions.

## 10. Final Incident Status

| Activity | Status |
|---|---|
| Initial service verification | Successful |
| Failure injection | Successful |
| Failure detection | Successful |
| Service restart | Successful |
| Recovery verification | Successful |

**Final Status: RESOLVED**

The manual chaos test was successfully completed. Nginx was stopped intentionally, its unavailability was verified, and normal service operation was restored through a manual restart.
