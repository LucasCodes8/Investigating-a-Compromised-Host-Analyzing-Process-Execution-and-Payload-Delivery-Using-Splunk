# Investigating a Compromised Host: Analyzing Process Execution and Payload Delivery Using Splunk

This lab investigates a potential security breach flagged by an IDS in the HR department, using Splunk to analyze process execution logs, identify malicious activities, and trace payload downloads.

---

## Table of Contents
- [Objective](#objective)
- [Lab Environment](#lab-environment)
- [Tools and Technologies](#tools-and-technologies)
- [Step-by-Step Process](#step-by-step-process)
  - [Step One: Identifying an Imposter Account](#step-one-identifying-an-imposter-account)
  - [Step Two: Finding Users Running Scheduled Tasks](#step-two-finding-users-running-scheduled-tasks)
  - [Step Three: Tracing Payload Downloads](#step-three-tracing-payload-downloads)
  - [Step Four: Analyzing the Malicious URL](#step-four-analyzing-the-malicious-url)
- [Results and Analysis](#results-and-analysis)
- [Challenges Faced](#challenges-faced)
- [Conclusion](#conclusion)
- [References](#references)
- [Full Lab](https://github.com/LucasCodes8/Investigating-a-Compromised-Host-Analyzing-Process-Execution-and-Payload-Delivery-Using-Splunk/blob/main/Investigating%20a%20Compromised%20Host_%20Analyzing%20Process%20Execution%20and%20Payload%20Delivery%20Using%20Splunk.pdf)

---

## Objective

This lab aims to:
1. Investigate suspicious process executions flagged by an IDS.
2. Identify compromised accounts and malicious actions.
3. Determine the use of LOLBins to download payloads.
4. Analyze the source of the malicious payload and retrieve necessary flags.

---

## Lab Environment

- **Network Segments**: IT, HR, and Marketing departments.
- **Logs**: Process execution logs (Event ID 4688) from the HR department hosts.
- **Tool**: Splunk for filtering and analyzing log data.

---

## Tools and Technologies

- **Splunk**: Log management and analysis.
- **Windows Event Logs (Event ID: 4688)**: Process creation and execution logs.
- **CertUtil**: A Windows system utility (LOLBIN) used to download the malicious payload.
- **File-sharing platforms**: Hosts the payload involved in the attack.

---

## Step-by-Step Process

### Step One: Identifying an Imposter Account

- Set the Splunk search query for March 2022 in the `win_eventlogs` index.
- Filter and list all active usernames.
- Discovered an imposter account using the alias `Amel1a`, attempting to mimic a legitimate user.

### Step Two: Finding Users Running Scheduled Tasks

- Filter the logs for HR users executing **Schtasks.exe**.
- Cross-reference with the employee list to identify **Chris.fort** as the only HR user running scheduled tasks.

### Step Three: Tracing Payload Downloads

- Investigate the user executing system processes to download a payload using Event ID 4688.
- Found **Haroon** using **certutil.exe** to download **benign.exe** from `controlc[.]com` on March 3, 2022, from the host **HR_01**.

### Step Four: Analyzing the Malicious URL

- Analyzed the URL: `https://controlc[.]com/e4d11035`.
- Retrieved the content of the payload and the required flag from the investigation.

---

## Results and Analysis

- **Imposter Account**: `Amel1a` was flagged as suspicious.
- **Scheduled Tasks**: **Chris.fort** was the only HR member running scheduled tasks.
- **Payload Download**: **Haroon** used **certutil.exe** to download a malicious payload from a remote server.
- **Payload Source**: The file-sharing platform **controlc[.]com** hosted the payload that compromised the HR host.

---

## Challenges Faced

- Sifting through large volumes of logs required precise filtering.
- Identifying **certutil.exe**, a legitimate tool, as a means to bypass security controls posed an initial challenge.

---

## Conclusion

The investigation revealed a compromised HR host due to an imposter account and unauthorized usage of system utilities to download a malicious payload. By leveraging Splunk's log filtering, we successfully identified the attack path and mitigated the threat.

---

## References

- [Splunk Documentation](https://docs.splunk.com/Documentation/Splunk)
- [Ultimate Windows Security - Event ID 4688](https://www.ultimatewindowssecurity.com)
- [Microsoft Docs - CertUtil](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/certutil)
