---
title: "Part1 - Planning & Use Case Management Process"
classes: wide
header:
  teaser: /assets/images/soc/part1/use_cases_score.png
ribbon: DodgerBlue
description: "The first part of building a SOC-CMM compliant MSSP"
categories:
  - SOC
toc: true
---

# Part 1 - SOC-CMM Planning & Use Case Management Process

## Back Story

I decided to start a series of blog posts that will be dedicated to creating a `full functional MSSP` starting from documentations, process, plans, and technical steps to run different departments inside this MSSP efficiently based on `SOC-CMM Framework` guidelines "If you don't know what is that, don't worry will discuss it later", this is not going to be things that you see in courses or and `I am not going to use AI at all` in writing this, this will make it takes time, but it will cover things that no course or AI will tell you about.



## Before We Go

First, let me mention that this is not and will never be a linear process, we will jump between departments and corelate the work between them and make sure our processes will be able to follow along with the intervention between the different responsibilities, also consider that no process is stander and every one should follow, process can differ but at the end they are required to deliver the same result or as better as it can.

In addition, I have a development team that will help me through this series to create an automated ways of doing things to make the life easier, let's see how it will go.


## SOC-CMM Framework

For every service any company provides there should be some requirements they should meet, some regulations they should stick to, and some measures to measure their performance and be able to find the path to be better on the service they are providing, this in short words what `compliance` is.


For our services there is a framework called [SOC-CMM](https://www.soc-cmm.com/), this framework is used to measure mainly two things in our environment:

- **Maturity:** this is a measurement for how each service is documented completely and how the processes are effective and covering all the aspects that should be covered.

- **Capability:** this is a measurement for how powerful are the services in short term "how far the the company can go with the service" in terms of technical capabilities.

How this works is that there is an Excel sheet you can download from [here](https://www.soc-cmm.com/downloads/soc-cmm%202.3.4%20-%20advanced.xlsx), this document will be in a form of questions and answers, and based on the answers a score will be calculated and given to each service and for the environment as all.

When visiting the Introduction page of the sheet we can find more information about the scoring and the levels available for this certification.

![alt text](/assets/images/soc/part1/SOCCMM_Intro.png)

For the areas covered in SOC-CMM here is the graph showing them:

![alt text](/assets/images/soc/part1/SOCCMM-Graph.png)

For us as technical stuff, we are going to focus on the `Process` and `Services` parts more, the technology part is more trialed for platform engineers but we will include that implicitly in our journey.


Let's have an example to see how we will work through that.

If we went to the capability section of the `Security Monitoring` service, we will find a list of questions that we are required to answer based on what we have in the environment "In our case as we are building with it, we will implement after reading it", the answers are not just `yes` or `no`, as there are multiple choices in between to make it more accurate, and this makes sense as in cyber security there is nothing that could be defined with only yes or no.

![alt text](/assets/images/soc/part1/SOCCMM-Example.png)


Also, if you looked at the end of each line you will find a description of what the question means to make it easier to go along with it.

![alt text](/assets/images/soc/part1/SOCCMM-Answers.png)

If you looked at th answers on the previous image, you can notice that there is a help for you to make the right choices, as when you choose an answer that will reflect a text, if what in that text is what you have this means your answer is right, if not this means that the answer is not accurate and you need to look more into it.


## MSSP Services

Before starting to implement anything we need to decide first what are the services that we are going to offer, and to decide that we need first to know, what are the services that could be possibly provided by an MSSP focusing on the `Defensive` side.


![alt text](/assets/images/soc/part1/Incident%20Responce%20services.png)


the previous draft I created trying to explain the relationship between the services, let me first try to explain what does a box inside another mean, this means that the service will require the other service mentioned inside it in a box to functional, maybe completely as the case between `Security Monitoring` and `Detection Engineering` because if there is no detection there is no point of monitoring, and maybe partially like the relation between `Detection Engineering` and `Threat Hunting`

    The relation between the services is a lot more than this, and too many overlapping and dependency on each other, this will be discussed in a more accurate way while creating the process for each department to define the relationship between them in a functional way

## What We Will Do

We will focus on the building blocks one by one and start enhancing from there, we will start the process from onboarding a client to providing a stable service for him, as said before this is not a linear process a department starts and another takes after it, the responsibilities are divided and multiple departments can have work at the same time.

For now I am planning to start with the following services:

- **Security Monitoring**
- **Security Incident Management**
- **Security Analysis & Forensics**
- **Log Management**


## First Step

Let's talk rationally about how business is going on this area, to start with onboarding a client and providing the service there are many prerequisites you should have on your environment to get accepted to be able to provide the service, for example, the first step on physically onboarding a client for the security monitoring service is to onboard logs from their environment to the SIEM solution so we can start running the operation of monitoring, but here comes the questions:

- **Do we have a list of logs that we will collect?**
- **Do we have detections implemented to catch anomalies on the collected logs?**
- **Do we have a plan in case on an alert?**...etc.


This is why the actual start should always be `Detection Engineering`, although it's not directly mentioned as a service in SOC-CMM but we can call it a `Sub-Service` for the security monitoring service and it's the core for the service, as the service flow starts with an alert, but from where that alert will come?!


## Detection Engineering In SOC-CMM


In SOC-CMM, Detection Engineering is listed under process, and it has two processes as following:

![alt text](/assets/images/soc/part1/DE-Processes-SOC-CMM.png)


These are the following explained in short:

- **Detection Engineering & Validation**: this stands for the process of managing a detection and making sure it follows all the requirements from validation, testing and tracking after deployment.

- **Use Case MManagement**: this stands for from where you know how much you are detecting and what detections should be created, and what is possible to detect and much more.


More in this will be discussed while implementing them.


## Tooling

As Discussed, I have a development team that will help me creating a product where we can collect all the mess in one place while we are going, for SOC-CMM tracking we agreed on the following functionalities.

![alt text](/assets/images/soc/part1/SOC-CMM-page.png)

this page will contain what is there on the sheet and we will drop our answers with evidence to them so we be able to track what we have and collect the evidence in one place while working, for us and for the team to be able to checkout on the platform if they wanted any clarifications on the processes or services.

![alt text](/assets/images/soc/part1/Page-SOC-CMM-Content.png)

The following features has been implemented:

- **Structured SOC-CMM Assessment Questions**
- **SOC-CMM Answers Tracking**
- **SOC-CMM Evidence Tracking Per Question**

## Use Case Management Process


From the description of both processes for Detection Engineering, it makes sense to start from the `Use Case Management` going to the `Detection Engineering & Validation`

Let's start creating a process that makes sense for us then drop it on the SOC-CMM questions to find if there are ways to make it better.


Let's take a carful look at the following flow:

![alt text](/assets/images/soc/part1/USE_Case_Mgmt_flow.png)


In this flow there are some parts that looks masteries which are `Heatmaps`, so let's discuss them and have clarity what they are as much as will be needed to understand the flow, after that we will have deep explanation on them while implementing them.

## Detection Heatmaps

Heat Maps are a Visualization of MITRE metrics with customization for every technique by assigning different scores and coloring scheme to be able to use it to specify a goal for our detection program, and track the progress to that goal.

We can't set our goal to be covering everything in MITRE, as this is near to impossible and all the possible ones will take a very long time that will consume the program before it can start, this is why there are different types of heat maps that should be created at the beginning of any planing before starting the actual research and implementation parts for detection rules.

So let's discuss what are the types of Heatmaps

## Attacker heatmap 

Attacker heatmaps represent a prioritized list of attack techniques that are most relevant to the organization. Cyber Threat Intelligence plays a vital role in identifying the attackers that are most relevant. The next step is to enumerate the attack techniques most commonly used by those attackers. The most common techniques used by any attacker is also included. Additionally, the techniques present in common attack tools and abuse by malware are added. Finally, CTI can be used to further priorities the list based on active campaigns.

## Defensive heatmap 

A defensive heatmap represent a prioritized list of the mitigating value that implemented security controls have for attack techniques. Attack techniques facing strong security controls are less likely to succeed. Missing controls, or controls that have a lower implementation level (for example, due to coverage) provide a much higher opportunity for abuse to their respective attack techniques. The protection heatmap helps to understand what attack techniques are more dangerous to the organization. That information can be used to priorities detection: detect where prevention is most likely to fail.

## Visibility heatmap 

A visibility heatmap represents the potential attack techniques that you could be monitoring for, based on the availability and quality of data sources. This heatmap provides insight into visibility. Data sources that have less coverage, are not available, or are not connected to the security monitoring platform are candidates for improvement.

## Detection capability heatmap 

A detection capability heatmap represents your current detection capability. Detection heatmaps are increasingly being embedded into security monitoring products. For example, in SIEM systems, such a heatmap is possible by tagging detection rules with MITRE ATT&CK® identifiers.

## Monitoring heatmap

The monitoring heatmap represents the actual rules that were triggered over a defined period of time. This heatmap can show what attack techniques are most common in your environment, and what techniques are rare or even never seen. This provides valuable input to establish your organization-specific attack patterns.


For us in the start it makes sense to start with `Attacker Heatmap`, so we can understand what we are defending against to know how we will plan our defense, from there we move to `Defensive Heatmap`, where we start knowing what our security tools can detect for us so we can focus on what they can't, then move to `Visibility Heatmap`, where we will map the log sources that we need to cover the techniques we are defending against, from there to the `Capability Heatmap` which will till us about our current status,ending with the `Monitoring Heatmap` which will tell us how good we are and what we should be attention to more.

But here comes a requirement, which could be descried as following:

There is no detection that can fit fully in the same way between different environment, the detection pipeline should be customized based on each client because every client will have a different `Attacker Heatmap`, which will affect the rest of the maps, and each client will have different security tools, which will affect the `Defensive Heatmap` which will also affect the rest of the maps.


For the previous reason, we need to choose a fictional client that we are going to explain the process for, I will choose this client to be a `Government Entity Operating In UAE`, we will consider that the following security solutions are on place:

- **Firewall**
- **EDR**

## Required Documents

Back to our flow, here is it the flow image again:

![alt text](/assets/images/soc/part1/USE_Case_Mgmt_flow.png)

it may seem a bit clearer now, but let's start implementing the required documents or files that needs to be there for the process to be able to run, then we can describe a full example after completing the process requirement for how to follow it.

## Creating Attacker Heatmap

## Break Down

let's break down the definition and start working on it based on our client that will get benefit from the program as we choose earlier.

`Cyber Threat Intelligence plays a vital role in identifying the attackers that are most relevant` this is the main part for creating the `Attacker Heatmap` for prioritizing the work, so let's start talking about how we can do that.

Our program as discussed is targeting a government entity operating in UAE region, the first step is to see what threat intelligence can provide us to start looking for `Threat Actors` who are interested in targeting our client.

    Note:
        we are not going to cover how we can create a threat intelligence program for that maybe this will be later, instead we are going to use publicly available free intelligence to build a profile for which threat actors we should track by our detections.
        
        If you have paid intelligence platform this may help you in the process instead of needing to go around in multiple places to collect as much as you can, so if you have it, use it.


Let's start listing some places where we can collect the groups that are interested in our client's business.

## Intelligence Sources

[ETDA](https://apt.etda.or.th/cgi-bin/aptsearch.cgi):

Serving the previous URL, we will be presented by a search functionality, let's put our needed search criteria as following:

![Image](/assets/images/soc/part1/APT-SearchETDA.png)

This will give us a list of threat actors which is what we are looking for as following:

![Image](/assets/images/soc/part1/APTLISTETDA.png)


Another good source could be the following:

[APT Map](https://andreacristaldi.github.io/APTmap/):


![Image](/assets/images/soc/part1/APTMAP_Search.png)

![Image](/assets/images/soc/part1/APTMAP_LIst.png)


## TTPs Collection

As said before this topic is more a threat intelligence department work, but if we don't have such a department we can still do it manually from free sources or it will be provided to us by paid service, so what we do in this step is to collect the techniques that was observed to be used by the treat groups we found in the previous step.

We can use different open source sites for that and a list of them are the following:

[MITRE Groups](https://attack.mitre.org/groups/)

[ETDA](https://apt.etda.or.th/cgi-bin/aptsearch.cgi)

[APT Map](https://andreacristaldi.github.io/APTmap/)

[Excel Sheet](https://docs.google.com/spreadsheets/d/1H9_xaxQHpWaa4O_Son4Gx0YOIzlcBWMsdvePFX68EKU/edit?gid=1864660085#gid=1864660085)

Note: If you have more sources to suggest, reach out on Linkedin [here](https://www.linkedin.com/in/amr-ashraf-93bb9a1ba/)


We should spend some time on this process if we don't have a threat intelligence program that provides us with this information, so we can collect as much Groups and TTPs that are likely going to be targeting our client.

The list of groups that we are going to focus on for our fictional client are listed below with a total number of 26 APT group.

```yml
# Groups Under Coverege "listing with different names"

- APT 1, APT1, Brown Fox, BrownFox, Byzantine Candor, Byzantine Hades, COMMENT PANDA, Comment Crew, Comment Group, G0006, GIF89a, Group 3, Operation “Oceansalt”, Operation “Seasalt”, Operation “Siesta”, PLA Unit 61398, ShadyRAT, Shanghai Group, TG-8223

- APT 28, APT-C-20, APT28, ATK 5, ATK5, Blue Athena, BlueDelta, FANCY BEAR, FROZENLAKE, Fancy Bear, Fighting Ursa, Forest Blizzard, G0007, Grey-Cloud, Grizzly Steppe, Group 74, IRON TWILIGHT, ITG05, Operation “DealersChoice”, Operation “Dear Joohn”, Operation “Komplex”, Operation “Pawn Storm”, Operation “Russian Doll”, Pawn Storm, SIG40, SNAKEMACKEREL, STRONTIUM, Sednit, Sofacy, Swallowtail, T-APT-12, TA422, TAG-0700, TG-4127, Tsar Team, UAC-0028

- APT 29, APT29, ATK 7, ATK7, Blue Kitsune, BlueBravo, COZY BEAR, Cloaked Ursa, CloudLook, Cozy Bear, Dark Halo, G0016, Grizzly Steppe, Group 100, IRON HEMLOCK, ITG11, Iron Ritual, Midnight Blizzard, Minidionis, Nobelium, Operation “Ghost”, Operation “Office monkeys”, Operation “StellarParticle”, SeaDuke, SilverFish, SolarStorm, StellarParticle, TA421, The Dukes, UNC2452, YTTRIUM

- APT 39, APT39, COBALT HICKMAN, Chafer, G0087, ITG07, REMIX KITTEN, Radio Serpens, TA454

- AQUATIC PANDA, BRONZE UNIVERSITY, CHROMIUM, Charcoal Typhoon, ControlX, Earth Lusca, FISHMONGER, Red Dev 10, Red Scylla, RedHotel, TAG-22

- BackDip, BackdoorDiplomacy, CloudComputating, Quarian 

- Alibaba, Cleaver, Cobalt Gypsy, Cutting Kitten, G0003, Op Cleaver, Operation Cleaver, Operation “Cleaver”, TG-2889, Tarh Andishan

- Crimson Sandstorm, Curium, Houseblend, IMPERIAL KITTEN, Imperial Kitten, Marcella Flores, Operation “Fata Morgana”, TA456, Tortoiseshell

- APT-C-06, ATK 52, ATK52, CTG-1948, DUBNIUM, DarkHotel, Fallout Team, G0012, Higaisa, Karba, Luder, Nemim, Nemin, Operation “DarkHotel”, Operation “Daybreak”, Operation “Inexsmar”, Operation “PowerFall”, Operation “The Gh0st Remains the Same”, Pioneer, SIG25, Shadow Crane, T-APT-02, TUNGSTEN BRIDGE, Tapaoux, Zigzag Hail

- AQUATIC PANDA, BRONZE UNIVERSITY, CHROMIUM, Charcoal Typhoon, ControlX, Earth Lusca, FISHMONGER, Red Dev 10, Red Scylla, RedHotel, TAG-22

- EQGRP, Equation Group, G0020, Platinum Colony, Tilded Team

- DeathStalker, Deceptikons, Evilnum, Jointworm, Operation “Phantom in the Command Shell”, TA4563

- Cobalt Foxglove, Fox Kitten, Lemon Sandstorm, PARISITE, PIONEER KITTEN, Pioneer Kitten, Rubidium, UNC757

- ATK 120, COBALT LYCEUM, HEXANE, LYCEUM, Lyceum, Operation “Out to Sea”, Spirlin, siamesekitten

- APT-C-06, ATK 52, ATK52, CTG-1948, DUBNIUM, DarkHotel, Fallout Team, G0012, Higaisa, Karba, Luder, Nemim, Nemin, Operation “DarkHotel”, Operation “Daybreak”, Operation “Inexsmar”, Operation “PowerFall”, Operation “The Gh0st Remains the Same”, Pioneer, SIG25, Shadow Crane, T-APT-02, TUNGSTEN BRIDGE, Tapaoux, Zigzag Hail

- ATK 116, ATK116, Blue Odin, Clean Ursa, Cloud Atlas, G0100, Inception Framework, OXYGEN, Operation “Cloud Atlas”, Operation “RedOctober”, The Rocra

- APT 15, APT15, BRONZE DAVENPORT, BRONZE IDLEWOOD, BRONZE PALACE, BackdoorDiplomacy, CTG-9246, Flea, G0004, GREF, Ke3Chang, Lurid, Metushy, NICKEL, Nylon Typhoon, Operation “Ke3chang”, Operation “MirageFox”, Playful Dragon, Playful Taurus, Red Vulture, Royal APT, Social Network Team, VIXEN PANDA, Vixen Panda 

- APT 35, APT35, Charming Kitten, Cobalt Illusion, Cobalt Mirage, Educated Manticore, G0059, Magic Hound, Mint Sandstorm, Newscaster Team, Operation “BadBlood”, Operation “SpoofedScholars”, Operation “Thamar Reservoir”, Phosphorus, TA453, TEMP.Beanie, Tarh Andishan, Timberworm, TunnelVision, UNC788, Yellow Garuda 

- ALUMINUM SARATOGA, ATK 89, Extreme Jackal, G0021, Gaza Cybergang, Gaza Hackers Team, Gaza cybergang, Molerats, Moonlight, Operation Molerats, Operation “DustySky”, Operation “DustySky” Part 2, Operation “Molerats”, Operation “Moonlight”, Operation “SneakyPastes”, Operation “TopHat”, TA402, TAG-CT5

- ATK 51, ATK51, Boggy Serpens, COBALT ULSTER, G0069, ITG17, MERCURY, Mango Sandstorm, MuddyWater, Operation “BlackWater”, Operation “Earth Vetala”, Operation “Quicksand”, Seedworm, Static Kitten, T-APT-14, TA450, TEMP.Zagros

- APT 34, ATK 40, CHRYSENE, Chrysene, Cobalt Gypsy, Crambus, EUROPIUM, Greenbug, Hazel Sandstorm, Helix Kitten, IRN2, ITG13, OilRig, TA452, Twisted Kitten, Volatile Kitten 

- EQGRP, Equation Group, G0020, Platinum Colony, Tilded Team

- G0033, Poseidon Group

- ATK 86, Contract Crew, Silence, Silence group, TAG-CR8, TEMP.TruthTeller, WHISPER SPIDER

- Blue Callisto, BlueCharlie, Calisto, Cobalt Edgewater, Cold River, Nahr Elbard, Nahr el bared, Seaborgium, Star Blizzard, TA446, TAG-53 

- FruityArmor, G0038, Project Raven, Stealth Falcon

- APT 36, APT36, C-Major, COPPER FIELDSTONE, Earth Karkaddan, Green Havildar, Mythic Leopard, Operation C-Major, Operation “C-Major”, Operation “Honey Trap”, Operation “Transparent Tribe”, ProjectM, STEPPY-KAVACH, TEMP.Lapis, TMP.Lapis, Transparent Tribe

- Dancing Salome, DeftTorero, Lebanese Cedar, Volatile Cedar

- DEV-0193, FIN12, GOLD BLACKBURN, Gold Blackburn, Gold Ulrick, Grim Spider, ITG23, Operation “BazaFlix”, Periwinkle Tempest, TEMP.MixMaster, WIZARD SPIDER, Wizard Spider
```

## Heatmap Creation

for creating the heatmap on MITRE, we are going to use a tool from MITRE called [Attack Navigator](https://mitre-attack.github.io/attack-navigator/), to not make it this too long let's mention an existing good explanation for using this tool [here](https://www.youtube.com/watch?v=78RIsFqo9pM)

let's use what was explained to map our APT list to and create our `Attacker Heatmap`, and after creation we can import it to [Attack Navigator](https://mitre-attack.github.io/attack-navigator/) to see the final result.

![alt text](/assets/images/soc/part1/Attackerheatmap.png)

Now at this point, we have a list of techniques organized based on the priority which got count by how many APTs from our list are using each specific technique, with that we can say that we have done our `Attack Heatmap`

## Creating Visibility Heatmap

As Described before, visibility heatmap represents the current visibility we have over the environment, but we are onboarding a new client for our demonstration and there are no data sources we are collecting, so this will be created based on the detections that we will create for the attacker heatmap, or we can start filling it with the known high impotence data sources then we modify it whenever there is a change on the data sources that we are onboarding or offboarding.

for that we are going to use a tool called [deTT&CT](https://rabobank-cdc.github.io/dettect-editor/#/home), this tool will help us manage the datasources that we need for the entire infrastructure.

![alt text](/assets/images/soc/part1/deTT&ct.png)

we can start by moving to the datasources tab and start listing the platforms that our client is using.

![alt text](/assets/images/soc/part1/datasources.png)

then we can start listing all the datasources we want and the platforms from which we are collecting each datasource.


the following list has all the datasources available and how many techniques on mitre could be detected using them.

```
Count  Data Source
--------------------------------------------------
255    Command Execution
206    Process Creation
98     File Modification
88     File Creation
82     Network Traffic Flow
78     OS API Execution
70     Network Traffic Content
58     Windows Registry Key Modification
58     Network Connection Creation
55     Application Log Content
50     Module Load
46     File Access
46     Web [DeTT&CT data source]
37     File Metadata
32     Logon Session Creation
26     Script Execution
22     Response Content
21     Internal DNS [DeTT&CT data source]
20     User Account Authentication
18     Process Access
17     Windows Registry Key Creation
17     Email [DeTT&CT data source]
15     Service Creation
15     Host Status
13     Active Directory Object Modification
12     Service Metadata
11     Process Metadata
10     Driver Load
10     File Deletion
9      Firmware Modification
9      Logon Session Metadata
9      Process Modification
8      User Account Metadata
7      Windows Registry Key Access
7      Scheduled Job Creation
7      Malware Metadata
7      Active Directory Credential Request
6      Container Creation
6      Web Credential Usage
6      Response Metadata
6      User Account Creation
6      Drive Modification
6      User Account Modification
5      Instance Creation
5      Active DNS
5      Passive DNS
5      Network Share Access
5      Drive Access
5      Service Modification
4      Image Creation
4      Instance Start
4      Active Directory Object Creation
4      Malware Content
4      Social Media
4      Domain Registration
4      Drive Creation
4      Windows Registry Key Deletion
3      Active Directory Object Access
3      Instance Metadata
3      Container Start
3      Web Credential Creation
3      Firewall Rule Modification
3      Firewall Disable
3      Instance Deletion
3      Snapshot Creation
3      Process Termination
2      Cloud Storage Enumeration
2      Cloud Storage Access
2      Pod Metadata
2      Active Directory Object Deletion
2      Cloud Service Modification
2      Cloud Service Disable
2      Certificate Registration
2      Cloud Storage Metadata
2      Instance Modification
2      Instance Stop
2      Firewall Metadata
2      Firewall Enumeration
2      Group Enumeration
2      Group Metadata
2      Image Metadata
2      Scheduled Job Metadata
2      Scheduled Job Modification
2      Kernel Module Load
2      WMI Creation
2      Group Modification
2      Driver Metadata
2      Snapshot Modification
2      Snapshot Deletion
2      Volume Deletion
2      Cloud Storage Modification
2      Cloud Service Enumeration
1      Cluster Metadata
1      Container Enumeration
1      Container Metadata
1      Pod Enumeration
1      Pod Creation
1      Pod Modification
1      Instance Enumeration
1      Snapshot Metadata
1      Snapshot Enumeration
1      Volume Metadata
1      Volume Enumeration
1      Named Pipe Metadata
1      User Account Deletion
1      Image Modification
1      Volume Creation
1      Volume Modification
1      Cloud Storage Creation
1      Cloud Service Metadata
1      Image Deletion
1      Cloud Storage Deletion
1      DHCP [DeTT&CT data source]
```

keep in mind that having less number of techniques doesn't mean the datasource is worse, this depends on a lot of other factors also like the priority of the techniques they are detecting based on our attacker heatmap, how good it's in detecting the technique it supposed to detect, and how easy it's to be bypassed.

After creating the datasources we are collecting or the ones we are planning to collect at this step we choose the save option, this will generate a yaml file, using the deTT&CT tool it self [here](https://github.com/rabobank-cdc/DeTTECT.git), we can convert that yaml file into a json file that we can upload to visualize in [Attack Navigator](https://mitre-attack.github.io/attack-navigator/) as before.


we can just use the following lines two download and install:

```bash
git clone https://github.com/rabobank-cdc/DeTTECT
pip3 install -r requirements.txt
```

then we can use the following command to convert the yaml file to json file for visualization in navigator

```bash
python3 dettect.py ds -fd sample-data/data-sources-endpoints.yaml -l
```

Also, consider that you can open the editor locally by the python tool using the following command

```bash
python3 dettect.py e
```
I created a target visibility and converted it to the following map:

![alt text](/assets/images/soc/part1/Visibility_Map.png)


## Creating Template Files

By now we have from the documents used in the process we created before, the `Attacker Heatmap` and `Visibility Heatmap`, now we will start creating the rest of the documents required for the process to run.


## Detection Research Template

This template is designed to ensure comprehensive and structured documentation of adversary techniques, it doesn't mean that the headers here are the only ones that could be used additional headers could be added based on requirements of specific techniques as well.


###### Technique Details

A brief summary of the adversarial technique, including the MITRE ATT&CK ID, tactic, and general behavior.

Example:

![Image](/assets/images/soc/part1/Research_details.png)

###### Required Logs & Fields

A list of MITRE Data sources and specific fields required to detect this technique (e.g., Sysmon, Windows Event Logs).

Example:

![Image](/assets/images/soc/part1/Logs_Required.png)

###### Technique Description

In-depth explanation of the technique’s mechanics, how it works under the hood.


###### Use in Adversary Operations

How adversary are abusing this technique to achieve the tactic that they are targeting..

###### Target Platforms

Platforms that can be affected (e.g., Windows, Linux, macOS), with any OS-specific behaviors noted.

###### Configuration Best Practice

Recommendations to harden systems and reduce the risk or impact of the technique (e.g., group policy settings, registry changes, access controls).

###### Abuses

Known misuses that attackers are using for this technique, how they do it, and how we can detect each misuse by creating a detection rule for it.


## Triage Playbook Template

The triage playbook should consist of questions that cover at least the following aspects around the triggers of the rule:

- **Context Collection**
- **Validation**
- **Correlation**
- **Analysis**
- **Risk Assessment**
- **Escalation**


Name of the triage playbook should be as following:

```
<TechniqueID>-<UniqueID>-<TechniqueName>.md
```

## Detection Rules Guide

###### Rule naming convention should be in the following format:

Example:

PER-E-54864-100-Default Account Enabled.yml

Described as following:

- Technique short name

```
REC - Reconnaissance
RSD - Resource Development
INA - Initial Access
EXE - Execution
PER - Persistence
PRE - Privilege Escalation
DFE - Defense Evasion
CRA - Credential Access
DIS - Discovery
LAT - Lateral Movement
COL - Collection
CNC - Command and Control
EXF - Exfiltration
IMP - Impact
```

- Log Source

The source type of the log

```
E - Endpoint
N - Network
```

- Unique ID for the rule

- Triage Playbook ID

- Detection Name



###### Detection Name

Provide a descriptive name for the detection rule.

Example: Suspicious PowerShell Command with Encoded Payload


###### Detection Description

Describe the purpose of this detection. What threat or malicious behavior does it aim to detect?
Example: Detect execution of base64-encoded PowerShell commands that could indicate phishing or malware delivery.


###### Status

Tracking the status of the rule from stability point of view:
- experimental.
- test
- stable


###### Threat Mapping

MITRE ATT&CK Technique(s): e.g., T1059.001 – PowerShell
Kill Chain Phase: e.g., Execution, Initial Access


###### Data Sources

Data source needed for the detection.
- MITRE Data Source
- Product targeted by the Rule
- Service in the product that provides the log

###### Detection Logic

Include the logic used in Sigma, Splunk SPL, KQL, or your SIEM-specific language. Add any filters or tuning techniques used.

Example (Sigma Rule):

```
detection:
  selection:
    EventID: 4104
    ScriptBlockText|contains|all:
      - 'powershell'
      - 'EncodedCommand'
  condition: selection
```

###### False Positive Considerations

List common legitimate scenarios or internal tools that might trigger this rule. Include known safe processes and ways to reduce false alerts.


###### Severity and Risk Rating

Severity Level: High / Medium / Low


###### References

Any external threat reports, MITRE links, or blogs you used to build or test this detection.


###### Author

Name of the rule author.


An Example sigma rule will be like the following:

```yaml
//PER-E-91101-100-Enable Default Windows Accounts via WMIC
title: Enable Default Windows Accounts via WMIC
status: test
description: Detects the activation of default Windows accounts (Guest, Administrator, DefaultAccount) using WMIC.
author: Amr Ashraf
date: 2025-03-28
references:
    - https://attack.mitre.org/techniques/T1078/001/ 
logsource:
    category: Command Execution
    product: windows
    service: sysmon
detection:
    selection:
        Image|endswith:
            - '\wmic.exe'
        CommandLine|contains:
            - 'useraccount'
            - 'where'
            - 'name'
    accounts:
        CommandLine|contains:
            - 'Guest'
            - 'DefaultAccount'
            - 'Administrator'
    condition: selection and accounts
falsepositives:
    - Legitimate system administration activity
level: high
tags:
    - attack.persistence
    - attack.initial_access
    - attack.t1078.001
    - attack.t1098
```


## Final Process & Procedures

By now, all the documents required for the process we created previously are ready so we can start describing from the graph how the process will go:


![Image](/assets/images/soc/part1/USE_Case_Mgmt_flow.png)

1. The process starts with two triggers, prioritized technique from Attacker Heatmap one after the other, or a custom request to create specific use case.

2. Based on the score we assign a technique or use case to a detection engineer.

3. The first step is to check the datasource requirements for the technique in the visibility heatmap.

4. If the required coverage isn't there, we raise a request to the platform engineering team to onboard the required logs, if required log sources are there, jump to 6.

5. If log sources can't be onboarded so the technique can't be researched and go to 1, if the log sources required onboarded successfully we update the visibility heatmap.

6. start following the research template for the assigned technique.

7. Create the detection rules possible from the research that has been done, following the detection rules template.

8. Create Triage playbook for the rules created following the triage playbook template.

9. Escalate to the Detection Engineering & Validation process.


## Tooling

So to extend the tool to cover that part, we created two pages, the first one is to track all the processes that we created and will create, because processes are being put to be followed so they should be somewhere where every one can see them and revise them.

![alt text](/assets/images/soc/part1/P&P.png)

The following features has been implemented:

- **Insert New Proccesses**
- **Processes Contains Image & Text**
- **MarkDown Format Supported**
- **Edit And Delete Functionality For the Processes**

The second page will track the Heatmaps that we are creating and give us the ability to display them and sort based on scoring.

![alt text](/assets/images/soc/part1/heatmaps_page.png)

The following features has been implemented:

- **Add & Remove JSON Heatmaps**
- **Display Heatmaps**
- **Reflecting Coloring Based on Score**
- **View Score on hover**
- **Filter based on Score**

## Use Case Management - SOC-CMM Score

Now let's start with SOC-CMM assessment questions to see what score we will get for the process that we just created.

- 4.1.1	Is there a use case management process or framework in place?


Fully, this is what we just created.


- 4.1.2	Are use cases formally documented?

Fully, based on SOC-CMM description for this point "Formal documentation may include use case documentation templates", templates are included in the process.

- 4.1.3	Are use cases approved by relevant stakeholders?

I will set the importance of this to None as it's not relevant here as there are no, that will not affect the score.

- 4.1.4	Is the use case management process aligned with other important processes?

Fully, the use case management process can accept input as a trigger from other departments, and it has steps of escalating to other processes like platform engineering or Detection & Validation.

- 4.1.5	Are use cases created using a standardized process?

Mostly, this is what heatmaps especially Attacker heatmap are doing here, the issue here is that there should be a Threat Intelligence service to make sure the relevant heatmaps are created a efficient as it could be and regularly updated, this is why we lose a point.


- 4.1.6	Are use cases created using a top-down approach?

Fully, this is descried in the document where we start with the heatmaps which reflects the risk and we go from there.


- 4.1.7	Can use cases be traced from high-level drivers to low-level implementation?

Fully, the templates created assures that this is easily tracked.

- 4.1.8	Can use cases be traced from low-level implementation to high-level drivers?

Averagely, this part is possible but requires a manual effort as not all individual rules can fit in a research document, some rules could be available open source so no need for research them and that makes them a bit hard to track as they are not created through the process.


- 4.1.9	Are use cases measured for implementation and effectiveness?

Mostly, this is what the research document is about, we lose a point here for the feedback loop as it will be implemented in the `Detection Engineering & Validation` process.

- 4.1.10	Are use cases scored and prioritized based on risk levels?

Fully, the process it self depends on privatization and scoring from the beginning.

- 4.1.11	Are use cases regularly revised and updated?

I will set the importance of this to None as it's not relevant here as the program is not running yet in real world, that will not affect the score.

- 4.2.1	Do you measure use cases against the MITRE ATT&CK® framework for gap analysis purposes?

Fully, the heatmaps are fully created based on MITRE Framework.

- 4.2.2	Are monitoring rules tagged with MITRE ATT&CK® framework identifiers?

Fully, The Rules template insures that.

- 4.2.3	Have you created a MITRE ATT&CK® risk profile for your organization?

Fully, this is what the attacker heatmap is for.

- 4.2.4	Have you prioritized MITRE ATT&CK® techniques for relevance?

Fully, The heatmaps insures that.

- 4.2.5	Is use case output (alerts) used in threat intelligence activities?

I will set the importance of this to None as it's not relevant here there is no Threat Intelligence program yet, that will not affect the score.

- 4.2.6	Is threat intelligence used for the creation and updates of use cases?

I will set the importance of this to None as it's not relevant here there is no Threat Intelligence program yet, that will not affect the score.

- 4.3.1	Do you determine and document visibility requirements for each use case?

Fully, research template and visibility heatmap ensures that.

- 4.3.2	Do you measure visibility status for your use cases for gap analysis purposes?

Fully, This is what Visibility heatmap is for.

- 4.3.3	Do you map data source visibility to the MITRE ATT&CK® framework?

Fully, This is what Visibility heatmap is for.


    For the points that we set to none in the importance selection, this doesn't mean that we can just put none, the threat intelligence service for example should exist we can't just ignore it, but we did that for now as we are trying to measure our current work without external factors that is not related to a non existing yet process, we will come to these points when we create those processes.


Based on our answers, our score for this process based on SOC-CMM is **4.17**

![alt text](/assets/images/soc/part1/use_cases_score.png)


At this point we can see we did a good job with the use case management process and we can continue our work to build the rest of our MSSP.

Next we will move to the `Detection Engineering & Validation` Process.