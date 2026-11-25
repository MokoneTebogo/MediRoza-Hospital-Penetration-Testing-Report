# 🔐 MediRoza Hospital — Penetration Testing Report

**Project:** `WK4-Penetration Testing Project` | **Program:** Cybersecurity at Networkwalks (`Batch B084`) | **Platform:** Kali Linux

## 📌 Assessment Information

| Item | Details |
| :--- | :--- |
| Pen Tester Name | Tebogo Mokone |
| Target | MediRoza Hospital Web Application |
| Assessment Type | Web Application Penetration Testing |
| Authorization | ✅ Written Permission Granted |
| Environment | Controlled Educational Environment | 
| Program | Networkwalks Cybersecurity |
| Platform | Kali Linux |
| Report | Detailed Penetration Testing Report |

> ⚠️ **Ethical Notice:** This assessment was conducted against an authorized target in a controlled educational environment. The techniques documented in this report must never be applied to systems without explicit written permission.

---

## Executive Summary

An authorized penetration test was conducted against the MediRoza Hospital web application to identify security weaknesses that could result in unauthorized disclosure of sensitive information.

The assessment included network and web reconnaissance, directory enumeration, web application testing, HTTP request analysis, PDF security testing, password recovery, and analysis of exposed server files.

The assessment identified three significant security issues:
1. Insufficient access control exposed three confidential patient laboratory reports.
2. Weak PDF password protection allowed the protected reports to be opened after password recovery.
3. An exposed database backup disclosed highly sensitive employee salary and hospital shareholder information.

The combined findings demonstrate weaknesses in access control, document protection, sensitive-file storage, and information exposure.

The overall security risk is considered **Critical** because the assessment ultimately demonstrated access to confidential medical, employee, financial, and corporate ownership information.

---

## Scope and Methodology

### Scope

Testing was limited to the authorized MediRoza Hospital environment and the activities required by the Networkwalks penetration-testing milestones.

#### In Scope
- MediRoza Hospital web application -- https://medirozahospital.com
- Publicly accessible services
- Web directories and files
- Patient portal functionality
- HTTP requests and responses
- Laboratory PDF reports
- Exposed server resources
- Information discovered during authorized testing

#### Out of Scope
- Denial-of-service attacks
- Destructive testing
- Modification or deletion of hospital information
- Systems outside the authorized target
- Public disclosure of confidential information

### Tools Used

| Tool | Purpose |
| :--- | :--- |
| Nmap | Port, service and entry-point discovery |
| DNSRecon / nslookup | DNS enumeration |
| Whois | Domain registration reconnaissance |
| WhatWeb | Web technology fingerprinting |
| cURL | HTTP request and response analysis |
| WAFW00F | Web Application Firewall detection |
| Dirsearch | Web directory and resource enumeration |
| Burp Suite | HTTP interception and application analysis |
| John the Ripper | Password recovery testing |
| HexStrike | Supporting security analysis |
| Kali Linux | Penetration-testing environment |

### Methodology

The assessment followed the following workflow:

```text
Reconnaissance
      ↓
Network & Service Enumeration
      ↓
Web Technology Analysis
      ↓
Directory / Resource Enumeration
      ↓
Patient Portal Investigation
      ↓
Burp Suite HTTP Analysis
      ↓
Patient Report Discovery
      ↓
PDF Security Analysis
      ↓
Password Recovery
      ↓
Document Access
      ↓
Exposed Backup Discovery
      ↓
Sensitive Data Analysis
```

### Limitations

- Testing was restricted to the authorized scope.
- No denial-of-service or destructive testing was performed.
- Some reconnaissance results produced blocked, filtered, redirected, or otherwise non-actionable responses and were therefore not treated as confirmed vulnerabilities.
- Sensitive information has been redacted from the public version of this report.

---

## Findings and Proof of Exploitation

### Confidential Patient Laboratory Reports

#### Description
The first milestone required the identification of three confidential patient laboratory reports.
Initial reconnaissance was performed to identify the target's externally accessible services and web application attack surface.
Nmap identified several exposed services and confirmed that HTTP/HTTPS services were available for further web application assessment.

#### Evidence — Network Reconnaissance
- **Nmap** — Used to identify open ports and exposed network services that could provide potential entry points into the target.

<img width="1919" height="1079" alt="nmap" src="https://github.com/user-attachments/assets/6a63242c-f271-4386-941c-565ab859235d" />


<br>
<br>
Further web reconnaissance was performed using tools including nslookup, DNSRecon, WhatWeb, cURL, and WAFW00F.
- **nslookup** — Used to resolve the target domain name to its associated IP address and verify DNS information.

<img width="1917" height="154" alt="nslookup" src="https://github.com/user-attachments/assets/392d914b-65e8-49fd-9ec7-5711635e7032" />

<br>
<br>
- **DNSRecon** — Used to enumerate DNS records such as nameservers, mail servers, IP addresses, SPF, and DMARC information.

<img width="1916" height="447" alt="dnsrecon" src="https://github.com/user-attachments/assets/672d16fa-c31a-4f2a-a5d7-c08f2cc366aa" />


<br>
<br>
These activities helped establish the target's DNS configuration, HTTP behaviour, server technologies, and available web attack surface.

#### Evidence — Web Reconnaissance
- **WHOIS** — Used to gather publicly available domain registration, registrar, nameserver, and domain-related information.

<img width="1919" height="1079" alt="whois" src="https://github.com/user-attachments/assets/ede8d857-c72e-4663-b39e-0c219c634c4c" />


<br>
- **WhatWeb** — Used to fingerprint the web application and identify technologies, server software, frameworks, and other web components.

<img width="940" height="332" alt="whatweb 1" src="https://github.com/user-attachments/assets/d0151e16-b413-4eae-8b83-0fecdd2b64ce" />


<br>
- **WAFW00F** — Used to determine whether the target web application was protected by a Web Application Firewall (WAF).

<img width="1913" height="361" alt="wafw00f" src="https://github.com/user-attachments/assets/e984f83f-b5c9-4f36-9082-e110302f84f2" />


<br>
- **cURL** — Used to send HTTP requests to the target and inspect the responses returned by the web server.

<img width="1919" height="1079" alt="curl" src="https://github.com/user-attachments/assets/9d0d1e2d-0dae-4a31-be2d-9a5e529062aa" />


<br>
- **cURL -I** — Used to retrieve and inspect HTTP response headers without downloading the full page content.

<img width="1907" height="192" alt="curl I" src="https://github.com/user-attachments/assets/96663b72-7f5c-4014-a182-0f1cfc38d097" />


<br>
- **cURL -v** — Used to view detailed HTTP connection information, including the request, response headers, TLS connection, and redirects.

<img width="1919" height="1079" alt="curl v" src="https://github.com/user-attachments/assets/9a9540e7-4767-4d95-b405-377d2b7e18da" />



<br>
#### Directory and Resource Enumeration
Web content enumeration was subsequently performed using Dirsearch.
The enumeration identified several accessible resources, including an `/old/` directory, which returned a successful HTTP response and required further investigation.

#### Evidence — Directory Enumeration
- **Dirsearch** — Used to enumerate hidden directories and files on the web server, helping identify exposed resources such as the `/old/` directory for further investigation.

<img width="1919" height="1079" alt="dirsearch" src="https://github.com/user-attachments/assets/1cbfccff-4476-4386-8015-fce7b0824ac6" />


<br>
Further investigation of the old resources identified an exposed backup associated with the application.

#### Evidence — Old Backup Discovery

<img width="1919" height="1079" alt="db_backup" src="https://github.com/user-attachments/assets/c6daa474-be99-4fbb-b648-681d37d9a7c9" />


<br>
#### Patient Portal Investigation
The patient-facing application functionality was investigated as part of the search for the required laboratory reports.
Burp Suite was configured to intercept browser traffic generated while interacting with the patient portal.

<img width="1919" height="1079" alt="burpsuite" src="https://github.com/user-attachments/assets/018649c8-93c3-49a4-b4a3-2145bb480352" />


<br>
#### Evidence — Burp Suite Interception
- **SQL Injection Testing** — Used to assess whether the application improperly handled user-supplied input in database queries and could potentially allow unauthorized database access.

<img width="1919" height="1079" alt="sql inject" src="https://github.com/user-attachments/assets/f754231e-8e2e-41d4-bf79-00df4c96b3fb" />


<br>
#### Successful Report Discovery
Testing resulted in access to a patient laboratory-report page containing the three PDF reports required by M1.

#### Evidence — Three Patient Reports
This confirmed that confidential patient laboratory documents were exposed through the tested application workflow.

<img width="957" height="1010" alt="Files" src="https://github.com/user-attachments/assets/8d0a2548-b73f-4a74-9bbd-0f5a7b6b52fa" />


<br>
> ⚠️ Patient names, medical information and other personally identifiable information must be redacted before this evidence is published publicly.

#### Impact
The vulnerability could allow unauthorized access to confidential medical documents.
Successful exploitation could result in:
- Disclosure of patient medical information
- Exposure of personally identifiable information
- Loss of patient confidentiality
- Privacy and regulatory consequences
- Reputational damage to the hospital

---

### PDF Password Security

#### Weak Password Protection of Confidential PDF Reports

##### Description
The laboratory reports obtained during M1 were protected by passwords.
Opening the reports resulted in a password prompt, confirming that PDF-level protection had been applied.

##### Hash Extraction
The password-protected PDF documents were processed using the available password-analysis tools.
Claude and Hestrike was used during the analysis of the protected reports.

##### Password Recovery
Password recovery testing was subsequently performed against the extracted password information.
The evidence demonstrates successful password recovery for the protected reports.
The tool installation/version was verified before the cracking process was performed.
- Evidence — Successful Password Recovery

<img width="794" height="641" alt="Passwords from JTR" src="https://github.com/user-attachments/assets/1d0b507c-902c-4692-8c77-c303671666b0" />


<br>
##### Successful Document Access
After recovering the required passwords, the protected laboratory reports could be opened.
- Evidence — Report 1 Opened

<img width="1919" height="1079" alt="report 1" src="https://github.com/user-attachments/assets/3ad9f084-8864-484a-9f8c-8d647926a710" />

<br>
- Evidence — Report 2 Opened

<img width="1919" height="1079" alt="report 2" src="https://github.com/user-attachments/assets/8c10e806-95fb-4fd7-a536-6a3b31ceafd7" />

<br>
- Evidence — Report 3 Opened

<img width="1919" height="1079" alt="report 3" src="https://github.com/user-attachments/assets/bab31562-66d7-4ebf-86e6-fe4f47c5fdd0" />


<br>
This demonstrated that the passwords protecting the confidential medical documents were not sufficiently resistant to the authorized password-recovery testing.

> ⚠️ Passwords and confidential medical information should be redacted from the public GitHub evidence.

##### Impact
Weak document passwords reduce the effectiveness of PDF-level protection.
If an unauthorized party obtained the PDF files, recoverable passwords could allow access to confidential medical information despite the documents being password protected.
PDF passwords should therefore not be treated as a substitute for proper application-level access control.

---

### Critical Sensitive Data Exposure

#### Exposed Internal Database Backup

##### Description
The third milestone required further analysis of information discovered during the previous phases.
Directory and file analysis identified an exposed historical database backup associated with the MediRoza Hospital application.
The exposed backup was identified as:

```text
mediroza_db_backup_2019.sql
```

The backup identified itself as an internal MediRoza Hospital database backup and contained confidential staff and shareholder records.
- Evidence — Backup Discovery

Analysis of the exposed database contents revealed two particularly sensitive datasets:
1. Hospital employee information and salary data
2. Hospital shareholder information

##### Employee Salary Exposure
The exposed staff database table contained employee information including:
- Employee name
- Job title
- Department
- Contact information
- Identification information
- Monthly salary
- Employment information

The assessment confirmed that salary records for 30 hospital employees were present in the exposed backup.
- Evidence — Employee Salary Data

<img width="1905" height="789" alt="Staff" src="https://github.com/user-attachments/assets/6abb50a0-4fc3-4b01-b682-7034d3d6d049" />


<br>
- Suggested filename:
- **cURL -s** — Used to send HTTP requests in silent mode, displaying the server response without progress or transfer information for cleaner analysis.
- Salaries will also be found on the `Stakeholders.txt` file

<img width="1918" height="149" alt="filenames" src="https://github.com/user-attachments/assets/1c08527f-d907-4d88-aa38-636d24b9ec4d" />


<br>
> ⚠️ National identification numbers, telephone numbers, email addresses and other unnecessary personal information must be redacted from the public evidence.

##### Shareholder Information Exposure
The same database backup contained a shareholders table.
The exposed information included:
- Shareholder names
- Ownership percentages
- Number of shares held
- Share class

The assessment identified 10 shareholder records within the exposed database.
- Evidence — Shareholder Data
Suggested filename:
- Shareholder and staff details will also be found on the `Stakeholders.txt` file

<img width="1918" height="443" alt="Shareholders" src="https://github.com/user-attachments/assets/fe9b9235-8010-41f4-bf1c-fb4a131ac907" />

<br>



##### Impact
Exposure of an internal database backup represents a severe confidentiality failure.
The exposed information could potentially facilitate:
- Targeted phishing
- Social engineering
- Employee impersonation
- Financial fraud
- Identity-related abuse
- Targeting of senior employees
- Corporate intelligence gathering
- Further attacks against hospital personnel

The combination of personal employee information, salary information and corporate ownership information significantly increases the potential impact of the exposure.

---

## 04 — Risk Rating

| ID | Vulnerability | Risk | Justification |
| :--- | :--- | :--- | :--- |
| M1-F1 | Insufficient Access Control to Patient Reports | 🔴 Critical | The application workflow resulted in access to confidential patient medical reports, directly compromising sensitive medical information. |
| M2-F1 | Weak PDF Password Protection | 🟠 High | Password protection on confidential medical reports was successfully defeated during authorized password-recovery testing. |
| M3-F1 | Exposed Internal Database Backup | 🔴 Critical | An accessible internal database backup exposed employee personal information, salaries and hospital shareholder records. |

**Overall Risk — Critical**

The overall assessment is rated Critical because multiple vulnerabilities resulted in demonstrated access to highly sensitive patient, employee and corporate information.
The combination of inadequate application access controls, weak document protection and exposed internal database information significantly increases the potential impact of compromise.

---

## Recommendations and Remediation

### Patient Report Access
The hospital should:
- Enforce server-side authentication and authorization for every patient report request.
- Verify ownership and authorization before returning any medical document.
- Prevent direct unauthenticated access to report files.
- Review patient portal session-management controls.
- Log and monitor access to medical reports.
- Perform regular access-control testing.

### PDF Password Protection
The hospital should:
- Avoid relying on PDF passwords as the primary protection for medical information.
- Use strong, unique and sufficiently complex passwords where document encryption is required.
- Implement application-level authorization before allowing documents to be downloaded.
- Store medical documents outside publicly accessible web directories.
- Review existing protected documents for weak passwords.
- Apply appropriate encryption for sensitive data at rest.

### Exposed Database Backup
The hospital should:
- Immediately remove database backups from web-accessible directories.
- Store backups in dedicated, access-controlled backup infrastructure.
- Review the web server for additional `.sql`, `.bak`, `.zip`, `.old`, and other backup files.
- Restrict backup access using least-privilege permissions.
- Encrypt sensitive backups.
- Remove unnecessary personal information from backup datasets where possible.
- Review access logs to determine whether the exposed backup has previously been accessed.
- Establish automated checks to detect sensitive files accidentally deployed to production web directories.

---

## 📊 Findings Summary

| Milestone | Finding | Evidence | Risk |
| :--- | :--- | :--- | :--- |
| M1 | Confidential patient reports accessible | 3 laboratory reports identified | 🔴 Critical |
| M2 | Weak PDF password protection | Protected documents successfully opened following password recovery | 🟠 High |
| M3 | Exposed database backup | Employee salary and shareholder information exposed | 🔴 Critical |

---

## 🏁 Conclusion

The penetration test identified multiple vulnerabilities affecting the confidentiality of sensitive information within the MediRoza Hospital environment.
M1 demonstrated access to three confidential patient laboratory reports.
M2 demonstrated that password protection applied to those reports could be defeated through authorized password-recovery testing, allowing the documents to be opened.
M3 identified an exposed internal database backup containing confidential employee salary information and hospital shareholder records.
The findings demonstrate the need for stronger server-side access controls, secure document handling, robust password policies and strict controls over database backups and other sensitive server files.
Remediation should prioritize the patient-report access-control vulnerability and exposed database backup because both provide direct access to highly sensitive information.

---

## ⚖️ Ethical Disclaimer

This project was conducted in a controlled environment for educational purposes only.
The target was authorized for security testing by Networkwalks.
All testing documented in this repository was performed within the authorized scope. These techniques must never be applied to systems without explicit written permission from the owner.
Patient, employee and organisational information should be redacted from all publicly published evidence.

---

## 👤 Author

Tebogo Mokone Cybersecurity Professional B084

LinkedIn: (https://www.linkedin.com/in/tebogo-mokone-ba2b35261/)

## 📌 Project Information

Program Name: Cybersecurity at Networkwalks | Week: 04 | Project: WK4-Penetration Testing Project | Repository: GitHub
