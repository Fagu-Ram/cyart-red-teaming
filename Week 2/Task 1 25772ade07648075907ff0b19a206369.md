# Task 1

# 1. Threat Hunting with Open-Source Tools

Step 1 : Open you windows VM and create a file called **powershell-sigma.yml**

Step 2 : Write the below code in that file.

```bash
title: Suspicious PowerShell Activity
logsource:
  category: process_creation
  product: windows
detection:
  selection:
    Image|endswith: '\powershell.exe'
    CommandLine|contains: '-Command'
  condition: selection
```

Step 3 : Type the below command to convert the sigma code to Elastic KQL query.

```bash
sigma convert -t lucene -p ecs_windows powershell-sigma.yml
```

![image.png](image.png)

Step 4 : Now go to **Security** > **Rules** > **Detection rules (SIEM)** > **Create Rule.**

![image.png](image%201.png)

Step 5 : Select the Custom query and under custom query, type the command.

```bash
process.executable:"*\\powershell.exe AND process.command_line:*\-Command*"
```

![image.png](image%202.png)

![image.png](image%203.png)

Step 6 : Click on “Create and enable the rule” button.

Step 7 : Run the below query in your windows machine and Now go to the **Discover Tab**  and filter it with **data_stream.dataset:windows.powershell.**

```bash
powershell -Command "Write-Host Test"
```

![image.png](image%204.png)

| Timestamp | Process | Command Line | Notes |
| --- | --- | --- | --- |
| Aug 21, 2025 @ 12:08:25.873 | powershell.exe | powershell.exe -Command Write-Host Test | Suspicious Execution |
| Aug 21, 2025 @ 12:08:25.779 | powershell.exe | powershell.exe -Command Write-Host Test | Suspicious Execution |
| Aug 21, 2025 @ 12:08:25.779 | powershell.exe | powershell.exe -Command Write-Host Test | Suspicious Execution |