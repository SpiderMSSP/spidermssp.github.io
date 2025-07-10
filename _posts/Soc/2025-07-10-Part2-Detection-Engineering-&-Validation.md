---
title: "Part2 - Detection Engineering & Validation Process"
classes: wide
header:
  teaser: /assets/images/soc/part2/score.png
ribbon: DodgerBlue
description: "The Second part of building a SOC-CMM compliant MSSP"
categories:
  - SOC
toc: true
---

# Part 2 - Detection Engineering & Validation


This is a continuation to the series, in [part1](https://spidermssp.github.io/soc/Part1-Introduction-&-Use-Case-Management-Process/), we talked about our plan, SOC-CMM, and Use Case Management process.

In this part we are going to talk about our second process `Detection Engineering & Validation`.

## Description

we previously mentioned that it stands for the process of managing a detection and making sure it follows all the requirements from validation, testing and tracking after deployment.

An important thing to mention here is that the detection process doesn't end by deploying a detection rules to the production, a full continuous process executes before and after that, a lot of feedback loops should be in place to track the status of the detection the ability to improve and the results coming out of it.


## Detection Engineering and Validation Process


![alt text](/assets/images/soc/part2/DEVProcess.png)

Before starting to describe the flow of the process, let's start with discussing the requirement that the process needs to be implemented.

## Detection Engineer Skills Requirements

The process showed before requires two main component, **Testing Environment**, and **Human Skills**, let's talk about the human skills part first.

To be able to execute the process completely in a good way, Detection Engineer should have the following skills:

1. MCSA level skills in AD management.
2. Linux administration capabilities.
3. OSCP level on understanding how attacker's operate.
4. High research ability.

## Testing Environment

The testing environment is the most important and critical part of the process, and as good, automated, and scalable the environment is, as good the results will be.

We are going to use a small environment like the following to test all our work on:

![alt text](/assets/images/soc/part2/Lab-ENV.png)

The environment has all the varieties we will need for this stage, we can later extend it to hae more products, but for now we have the following:

- Active Directory.
- Linux server.
- Windows workstation.
- EVR & MSSP required tools "we will talk about these tools later"


we will start by creating a clean installation for the following machines on VirtualBox:

- 2 Ubuntu 22.04.
- 1 Windows 10.
- 1 Windows Server 2022.

Installation process is very straight forward, if you don't know how to do it you can find a lot of tutorials on youtube, here are some random ones.

- [Ubuntu](https://www.youtube.com/watch?v=Hva8lsV2nTk)
- [Windows 10](https://www.youtube.com/watch?v=CMGa6DsGIpc)
- [Windows Server 2022](https://www.youtube.com/watch?v=idPJ_hOFgWY)


Before you start, make sure to create the same subnet we are going to use in Virtual box "10.0.2.0/24"

![Image](/assets/images/soc/part2/vboxsubnet.png)

### DC01

For configuration watch the following [Configuration Video](https://drive.google.com/file/d/1Fk5ovMZxnnSz0iDkZRPBUfvhPRFvEffy/view?usp=share_link)


### PC01


For configuration watch the following [Configuration Video](https://drive.google.com/file/d/1BhnQrRyG5gW_Ed0htuej_gRa-YFFsBcK/view?usp=share_link)


### Web Server


For configuration watch the following [Configuration Video](https://drive.google.com/file/d/1tzeXIDXlwwt4mhFV0HmKf8NpfnsVsdv7/view?usp=sharing)



**Commands used**
```bash
sudo apt update
sudo apt install apache2
sudo apt install openssh-server
```

For configuration watch the following video[Connecting To the Domain](https://drive.google.com/file/d/187wQZeZ-iWgLLJjMAFda1GXebaVNW6JW/view?usp=sharing)

### SIEM


For configuration watch the following [Configuration Video](https://drive.google.com/file/d/1aucmmUDraYE8I7R0NPtRuQYS0XwFhzfE/view?usp=sharing)


**Commands used**
```bash
sudo su
sudo apt update
sudo apt install openssh-server
sudo apt install curl
apt install default-jdk default-jre -y
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" > /etc/apt/sources.list.d/elastic-7.x.list
apt update
apt install elasticsearch -y
systemctl restart elasticsearch
systemctl enable elasticsearch 
apt install logstash
systemctl start logstash
systemctl enable logstash 
sudo apt install kibana
systemctl start kibana
systemctl enable kibana
```

**xpack configuration**
```
xpack.security.enabled: true
xpack.security.authc.api_key.enabled: true
#
xpack:
  security:
    authc:
      realms:
        native:
          native1:
            order: 0
```

### Logging Configuration

Then we need to configure a good logging policy and enable whatever logs we see we will need them, We will consider the scalability on applying our required logging policy on the environment to be able to scale it to as much endpoints as we can.

### Windows

For Windows we will use [Group policy](https://www.techtarget.com/searchwindowsserver/definition/Group-Policy) to deploy a PowerShell script that will be used to:

- Download and install [Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) that will provide us with additional visibility and logging mechanisms.

- enable PowerShell scrip block logging, which will log PowerShell activities.

- Increase the size of Security log file to 1 GB.

### Configuration Steps

You can find the script that we want to run via group policy here:

make sure to read the script and put the required files on the mentioned shares.

```ps1
# === Step 1: Check if Sysmon is already installed ===
$sysmonService = Get-Service -Name "Sysmon64" -ErrorAction SilentlyContinue

if (-not $sysmonService) {
    # === Step 2: Download Sysmon.exe and sysmonconfig.xml from share ===
    $sourceSysmon = "\\DC01\Share\Sysmon.exe"
    $sourceConfig = "\\DC01\Share\sysmonconfig.xml"

    $localSysmon = "$env:TEMP\Sysmon.exe"
    $localConfig = "$env:TEMP\sysmonconfig.xml"

    if (-not (Test-Path $localSysmon)) {
        Copy-Item -Path $sourceSysmon -Destination $localSysmon -Force
        Write-Host "Downloaded Sysmon.exe to $localSysmon"
    } else {
        Write-Host "Sysmon.exe already exists locally."
    }

    if (-not (Test-Path $localConfig)) {
        Copy-Item -Path $sourceConfig -Destination $localConfig -Force
        Write-Host "Downloaded sysmonconfig.xml to $localConfig"
    } else {
        Write-Host "sysmonconfig.xml already exists locally."
    }

    # === Step 3: Install Sysmon with config ===
    Start-Process -FilePath $localSysmon -ArgumentList "-accepteula -i `"$localConfig`"" -Wait -NoNewWindow
    Write-Host "Sysmon installed successfully with configuration from $localConfig"
} else {
    Write-Host "Sysmon is already installed. Skipping download and installation."
}

# === Step 4: Enable PowerShell Script Block Logging ===
$regPath = "HKLM:\Software\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging"
$loggingEnabled = $false

if (Test-Path $regPath) {
    $val = Get-ItemProperty -Path $regPath -Name "EnableScriptBlockLogging" -ErrorAction SilentlyContinue
    if ($val.EnableScriptBlockLogging -eq 1) {
        $loggingEnabled = $true
    }
}

if (-not $loggingEnabled) {
    if (-not (Test-Path $regPath)) {
        New-Item -Path $regPath -Force | Out-Null
    }
    Set-ItemProperty -Path $regPath -Name "EnableScriptBlockLogging" -Value 1
    Write-Host "PowerShell Script Block Logging has been enabled."
} else {
    Write-Host "Script Block Logging is already enabled."
}

# === Step 5: Increase Security Event Log Size to 1GB ===
$logName = "Security"
$desiredSizeKB = 1048576

$currentSizeOutput = wevtutil gl $logName | Select-String "maxSize"
$currentSizeKB = ($currentSizeOutput -split ":")[1].Trim()

if ([int]$currentSizeKB -lt $desiredSizeKB) {
    wevtutil sl $logName /ms:$desiredSizeKB
    Write-Host "Security Event Log size increased to 1GB."
} else {
    Write-Host "Security Event Log size already set to 1GB or higher."
}

# === Done ===

```


[Configuration Video](https://drive.google.com/file/d/1Q7mBXB9msqDY4_c8DVlCsHI1Kjl2Hv7j/view?usp=sharing)


Now whenever a machine added to that OU and restarted, it will execute the script and will be configured as we wanted.

![Sysmon Service](/assets/images/soc/part2/sysmon.png)


### Shipping Logs "SIEM"

To ship logs from the environment to our SIEM solution, we are going to use agent based approach, where on every machine there will be an agent installed to send the logs and we can control our logging configuration through it.


### Initializing ELK

first, we need to initialize our ELK with [fleet](https://www.elastic.co/blog/elastic-agent-and-fleet-make-it-easier-to-integrate-your-systems-with-elastic).

[ELK Fleet Initialization](https://drive.google.com/file/d/1F7peMBxInr6c-D0wojIswxpphUy55IU3/view?usp=sharing)

### Windows

Note: Make sure to use the agent version recommended on the fleet settings

[Windows Agent Installation](https://drive.google.com/file/d/1oeXwjDQxy82ngaEOVCX-H5GdaqYcT818/view?usp=sharing)

### Linux 

Note: Make sure to use the agent version recommended on the fleet settings

[Linux Agent Installation](https://drive.google.com/file/d/1p0JH4G94K5C54rkN6_AbIprfhS9JhXeh/view?usp=sharing)


At the end we will have our machines setup and sending to our SIEM solution whatever we want them to send and we will configure that later when we decide what we want to collect.

![agents](/assets/images/soc/part2/agents.png)


### EVR Installation

we are going to use [Velociraptor](https://docs.velociraptor.app/docs/server_automation/server_api/) as an EVR tool.

For the server installation, you can find the following [video](https://drive.google.com/file/d/1bqxoQN8e0ZvnJOEPlvfiknfC-PiaFVOy/view?usp=sharing)

**Content Used**

```bash
[Unit] 
Description=Velociraptor linux amd64 
After=syslog.target network.target 
 
[Service] 
Type=simple 
Restart=always 
RestartSec=120 
LimitNOFILE=20000 
Environment=LANG=en_US.UTF-8 
ExecStart=/usr/local/bin/velociraptor --config /etc/velociraptor/server.config.yaml frontend -v 
 
[Install] 
WantedBy=multi-user.target 
```

Now let's generate the client configuration file and show a demo of how we can install the client in both windows and linux machines as in the following [video](https://drive.google.com/file/d/161fpLaN8TCTrzSloJDwznEKANHPAnnAJ/view?usp=sharing)

**Content Used**

```bash
[Unit] 
Description=Velociraptor linux amd64 
After=syslog.target network.target 
 
[Service] 
Type=simple 
Restart=always 
RestartSec=120 
LimitNOFILE=20000 
Environment=LANG=en_US.UTF-8 
ExecStart=/usr/local/bin/velociraptor --config /etc/velociraptor/client.config.yaml client -v 
 
[Install] 
WantedBy=multi-user.target 
```

At the end we will see all our clients with velociraptor installed

![alt text](/assets/images/soc/part2/velo-ui.png)

at this part our testing environment is ready and we can continue to discuss about the process again and what procedures we are going to follow to deploy, attack, and test our detections.

## Procedures

As in the process chart again in the following image:

![alt text](/assets/images/soc/part2/DEVProcess.png)

we have two ways to start this process, the first one is from receiving new detection rules from the **Use Case Management** process we discussed on the first part, and the second start is to get a tuning request on rules that already exist.

1. New Detection Rules coming onboard request.

2. When receiving new detection rules to process in the Detection Engineering and validation process we need to inform analysts team that we are processing a new patch of rules for deployment, so they can expect that those will be in production soon and new playbooks are coming to review.

3. Then we start deploying these rules into our previously mentioned testing environment to start testing.

4. Then we start two testing steps, the first is by running an attack that should be detected by each rule to see if the rule will detect the attack, we can do that through automated tools or manual attack.

5. Observing the result, if the rule didn't fire we return it back to the "Use Case Management Process" if not we continue the process.

6. The second test is testing the rules against historical 1-3 months of logs to see if there are any false positives will be generated once we deploy the rule to tune them accordingly.

7. if there are too much false positives, we return the rules caused it too "USe Case Management Process" other wise we do the whitelisting.

8. then we can deploy the rules to production if the two tests based successfully.

For the tuning request processing:

1. Review the tunning request once received, if the report is not sufficient for tunning, reject and return to the requester.

2. If the request found needs tunning, check if there is an alert fatigue, means the alert is very noisy, if so and it's only one rule causing the issue, tune it.

3. If there is an alert fatigue from multiple rules, rule back to before deploying the last batch of rules because this indicates bad FP testing when first deploying the rule.

## SOC-CMM Scoring

Now, let's go over SOC-CMM questions and see what score we will get 

5.1.1	Do you have a detection engineering process in place?

    Fully, full process covering all cases

5.1.2	Is the detection engineering process formally documented?

    Fully, the previous sections represents the documentation.

5.1.3	Are there specific roles and requirements for detection engineers?

    Fully, the requirements for detection engineers is mentioned on the previous documentation.

5.1.4   Is there active cooperation between the SOC analysts and the detection engineers?

    Fully, the process ensures collaboration between SOC and detection engineers by managing tuning, and suggestion requests, in addition to pre informing of stacked detections.

5.1.5	Is there active cooperation between the Threat Intelligence analysts and detection engineers?

    The Threat Intelligence process is not implemented yet, so we will set the importance for this point to none till creating that process.

5.1.6	Are there formal hand-over to the analyst team?

    Fully, the analysts team gets informed once there are any rules stacked for the validation process to know what to expect in the environment, in addition to review the new playbooks for these rules.

5.1.7	Is there a testing environment to test and validate detections before deploying them?

    Fully, the environment is discussed throughout this documentation.

5.1.8	Is there a formal release process in place for new detections?

    Fully, the process ensure that deploying the rules to production goes through the previously mentioned process.

5.1.9	Do you apply a versioning system to detections?

    Mostly, the process is packing every deployment with version number before deploying to help in achieving the rollback functionality mentioned in the process in addition to using git to do so, but commit is not formalized.

5.1.10	Do you have a roll-back procedure in place in case of problems with detections?

    Fully, rollback is fully integrated with the process.

5.2.1	Do you perform adversary emulation or automated detection testing?

    Fully, part of the process mentioned before.

5.2.2	Do you test for detection of MITRE ATT&CK® techniques?

    Fully, the entire operation is based on Mitre starting with the use case management process

5.2.3	Do you test detection analytics not directly associated with MITRE ATT&CK®?

    Averagely, as this is more to threat intelligence and we don't have this capability yet, but this is being done from the custom requests to the use case management process.

5.2.4	Do you test response playbooks?

    Fully, creating playbooks is main part of the use case management process.

5.2.5	Is detection validation fully integrated in the detection engineering process / pipeline?

    No, Pipeline is a bit complected task that needs specific DEVOPS requirements that is not there in most of the teams, we will discuss building this fully in upcoming posts.

5.2.6	Is the outcome from detection validation used as input into monitoring and detection engineering?

    Fully, the validation part of the process ensures that the process has clear path in case of success or failure.

5.2.7	Do you monitor the data ingestion status for data sources?

    Mostly, most of the SIEM solutions has the ability to handle this internally resulting in alerts in case of issues.

5.2.8	Do you actively measure and improve data source coverage?

    Fully, the use case management process ensures that visibility heatmap is integrated fully in the process.

Based on our answers, our score is **4.38**, which is considered a great score, please know that this scoring is subjected to small decrease based on the intense of the review and the acceptance of the marked importance none questions.

![alt text](/assets/images/soc/part2/Score.png)
