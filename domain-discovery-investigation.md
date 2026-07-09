# Domain Discovery Activity Investigation

## Alert Information

| Field      | Value                                            |
| ---------- | ------------------------------------------------ |
| Date       | March 27, 2025                                   |
| Time       | 19:56                                            |
| Severity   | Medium                                           |
| Alert Name | Spike of Domain Discovery Commands               |
| Status     | Awaiting Action                                  |
| Verdict    | True Positive (TP)                               |
| Category   | Active Directory Discovery / Endpoint Compromise |

---

# Summary

A suspicious spike of Active Directory discovery commands was detected on the host `DMZ-MSEXCHANGE-2013`.

The alert identified commands commonly used by attackers after gaining access to an environment, including privilege discovery, domain group enumeration, and domain controller discovery.

The activity was determined to be suspicious due to the combination of domain reconnaissance commands, execution from a reverse shell, and elevated SYSTEM privileges.

---

# Alert Details

| Field               | Value                                |
| ------------------- | ------------------------------------ |
| Host Name           | DMZ-MSEXCHANGE-2013                  |
| Operating System    | Windows Server 2012 R2               |
| User Account        | NT AUTHORITY\SYSTEM                  |
| Source Process      | C:\Windows\System32\cmd.exe          |
| Parent Process      | C:\Users\Public\revshell.exe         |
| Grandparent Process | C:\Windows\System32\inetsrv\w3wp.exe |

---

# Commands Observed

| Command                             | Purpose                     | Attacker Use                        |
| ----------------------------------- | --------------------------- | ----------------------------------- |
| `dir`                               | Lists files and directories | Discover files and system contents  |
| `hostname`                          | Displays computer name      | Identify compromised host           |
| `whoami /priv`                      | Displays current privileges | Determine access level              |
| `net group "Domain Admins" /domain` | Lists Domain Admin accounts | Identify privileged users           |
| `nltest /dclist:tryhackme.thm`      | Lists domain controllers    | Map Active Directory infrastructure |

---

# Indicators Observed

| Indicator                   | Finding                             | Risk                             |
| --------------------------- | ----------------------------------- | -------------------------------- |
| Process Chain               | `w3wp.exe → revshell.exe → cmd.exe` | Possible web server compromise   |
| Reverse Shell               | `C:\Users\Public\revshell.exe`      | Possible attacker remote access  |
| Privileged Account          | `NT AUTHORITY\SYSTEM`               | Highest local privilege level    |
| Domain Discovery            | Domain Admin enumeration            | Possible attacker reconnaissance |
| Domain Controller Discovery | `nltest /dclist` usage              | Active Directory mapping         |

---

# Investigation

## 1. Process Tree Analysis

Observed process execution:

```
C:\Windows\System32\inetsrv\w3wp.exe
        |
        ↓
C:\Users\Public\revshell.exe
        |
        ↓
C:\Windows\System32\cmd.exe
        |
        ↓
Domain Discovery Commands
```

### Analysis:

`w3wp.exe` is a legitimate IIS web server worker process. However, it is unusual for an IIS worker process to launch a reverse shell and command prompt.

The process relationship indicates a possible compromise of the web server, resulting in command execution by an attacker.

---

## 2. Reverse Shell Investigation

Observed file:

```
C:\Users\Public\revshell.exe
```

### Analysis:

The `Users\Public` directory is commonly abused by attackers because it allows easy file placement and execution.

The presence of a reverse shell executable suggests an attacker may have gained remote access to the system.

---

## 3. Privilege Analysis

Observed user:

```
NT AUTHORITY\SYSTEM
```

### Analysis:

The SYSTEM account has extensive privileges on a Windows machine.

Attackers who obtain SYSTEM access may be able to:

* Access sensitive files
* Dump credentials
* Disable security controls
* Move laterally through the network

---

## 4. Active Directory Discovery Analysis

The attacker executed commands to gather information about the domain environment.

Examples:

### Domain Admin Discovery

```
net group "Domain Admins" /domain
```

Purpose:

Identify accounts with the highest level of Active Directory privileges.

---

### Domain Controller Discovery

```
nltest /dclist:tryhackme.thm
```

Purpose:

Identify domain controllers that manage authentication and directory services.

---

# MITRE ATT&CK Mapping

| Technique                                  | ID        | Description                                          |
| ------------------------------------------ | --------- | ---------------------------------------------------- |
| System Owner/User Discovery                | T1033     | Identify current user context                        |
| Permission Groups Discovery: Domain Groups | T1069.002 | Identify privileged domain groups                    |
| System Network Configuration Discovery     | T1016     | Gather network information                           |
| Remote Access Software / Command Shell     | T1059     | Execute commands through command-line interfaces     |
| Exploitation of Public-Facing Application  | T1190     | Possible initial access through exposed web services |

---

# Response Actions

## Completed Actions:

* Reviewed the domain discovery alert.
* Analyzed the process execution chain.
* Identified suspicious reverse shell activity.
* Verified commands used for Active Directory reconnaissance.
* Classified the alert as True Positive.
* Escalated the alert to L2 for further investigation.

## Recommended Actions:

* Isolate the affected host if compromise is confirmed.
* Investigate the IIS application for exploitation attempts.
* Analyze `revshell.exe` hash and origin.
* Review web server logs for suspicious requests.
* Search for additional compromised hosts.
* Reset credentials if credential exposure is suspected.

---

# Escalation Details

| Field                 | Value                                                                                                |
| --------------------- | ---------------------------------------------------------------------------------------------------- |
| Escalation Level      | L2 Analyst                                                                                           |
| Intermediate Verdict  | True Positive (TP)                                                                                   |
| Reason for Escalation | Possible web server compromise with SYSTEM-level access and Active Directory reconnaissance activity |

---

# Final Verdict

## True Positive (TP)

### Reason:

The alert was classified as a True Positive due to multiple indicators of compromise:

* IIS worker process spawning a reverse shell.
* Reverse shell executing commands as SYSTEM.
* Discovery of Domain Admin accounts.
* Discovery of domain controllers.
* Behavior consistent with post-compromise reconnaissance.

The activity indicates a likely compromised server requiring further investigation and containment.

---

# Lessons Learned

* Individual discovery commands may be legitimate, but context determines whether activity is malicious.
* Process relationships are critical when investigating endpoint alerts.
* Web servers are common targets because they are often exposed to external users.
* Attackers frequently perform Active Directory discovery after gaining access to understand the environment before moving laterally.
