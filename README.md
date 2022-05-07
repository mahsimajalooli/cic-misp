# CTIX Ingestion Connector - MISP (CIC-MISP)

## **Introduction**

CIC-MISP is a python script developed by Cyware to facilitate communication between the MISP and CTIX applications. It facilitates you to ingest CTIX data and view it on the MISP platform.

**Use CIC-MISP to:**

- **List all collections** shared on your TAXII Server.
- **Poll for all objects** from the CTIX application to MISP for a particular collection via TAXII 2.0.
- **Automatically poll for data** from CTIX into MISP by defining a **polling frequency**.
- **Import CTIX data into MISP** using MISP APIs.

**CTIX data in MISP**

Every indicator polled from CTIX is displayed as an individual event in MISP.

Every report object from CTIX is displayed as an individual event in MISP. If a report object is associated with any other objects such as campaigns, threat actors, tags, TLP, etc, they are extracted and displayed as Tags on the MISP instance. This creates a single all-in-one encompassing event per report.

**About CITX:**

CTIX (Cyware Threat Intelligence eXchange) is a smart, client-server threat intelligence platform (TIP) for ingestion, enrichment, analysis, and bi-directional sharing of threat data within your trusted network.

**About MISP:**

MISP is an open-source Threat Intelligence Platform that facilitates sharing, storing, and correlating information on Indicators of Compromise. It also provides comprehensive information about targeted attacks, threat intelligence, financial fraud information, vulnerability information, or counter-terrorism information.

## **Prerequisites**

To use this script you must have :

**TAXII server credentials from CTIX:**

- CTIX TAXII Discovery URL
- CTIX TAXII Subscriber Username
- CTIX TAXII Subscriber Password
- CTIX TAXII Collection ID

**Important** : You can only use the TAXII 2.0 discovery URL as this script works only with STIX 2.0 format.

**API credentials from MISP** :

You will need your **MISP URL** that specifies where you are hosting your MISP instance and the **API key**.

To fetch the MISP API key, sign in to your MISP instance and follow **Administration** > **List Auth Keys** > **Add Authentication Key**

## **Installation and Usage**

You can configure this script in your environment in the following ways.

- Poll for CTIX data one time
- Automatically poll for CTIX data

### **Poll for CTIX data one time**

Configure your data and run the script one time. It fetches the data from CTIX and uploads it to the MISP application as a one-time activity.

- Clone the repository from GitHub [git clone https://github.com/mahsimajalooli/cic-misp.git]
- Install the requirements mentioned in the requirements.txt file [source /var/www/MISP/venv/bin/activate] then [pip3 install -r requirements.txt]
- Configure the following required credentials into the credentials.py file.

taxii_discovery_url = &quot;Enter your TAXII 2.0 URL&quot;

taxii_username = &quot;Enter your TAXII 2.0 username&quot;

taxii_password = &quot;Enter your TAXII 2.0 password&quot;

taxii_collection_id = &quot;Enter your TAXII 2.0 collection name from which the data is polled from CTIX into MISP&quot;

initial_date_from = &quot;Enter the date from which you want to poll the data in the format YYYY-MM-DD&quot;

misp_url = &quot;Enter your MISP URL&quot;

misp_api = &quot;Enter your MISP API key&quot;
- Perform the following operations using the commands. Use the help command for displaying help text. (python3 main.py --help)

       List Indicators : List all your collections from your TAXII server.

       list collections in server: python3 main.py --option list_collections

       Poll for Indicators: Polls for all indicators and upload them to MISP. Each indicator is parsed and displayed as an event in the MISP application.

       run for polling indicators: python3 main.py --option poll_indicators

       Poll for Reports: Polls for all report objects from CTIX and uploads them to MISP. Every report object is parsed and displayed as an event in the MISP application. In case the report object is associated with any indicators, the script extracts the indicators from these report objects and then uploads them to your MISP instance within a single event effectively creating all in one encompassing event per report.

       run for polling reports: python3 main.py --option poll_reports

### **Automatically poll for CTIX data**

Configure your data and run the CIC-MISP script as a cron job so that it can periodically fetch the data from the CTIX application and upload it to MISP. You will need to define a polling frequency in seconds to periodically poll for data.

- Clone the repository from GitHub [git clone https://github.com/mahsimajalooli/cic-misp.git]

- Install the requirements mentioned in the requirements.txt file [source /var/www/MISP/venv/bin/activate] [pip3 install -r requirements.txt]

- Configure the following required credentials into the credentials.py file.

**Important** : You have to specify the cron job interval in seconds for the parameter cron_seconds to run the cron job successfully. It recommends a schedule for one or two poll requests per day.

taxii_discovery_url = &quot;Enter your TAXII 2.0 URL&quot;

taxii_username = &quot;Enter your TAXII 2.0 username&quot;

taxii_password = &quot;Enter your TAXII 2.0 password&quot;

taxii_collection_id = &quot;Enter your TAXII 2.0 collection name from which the data is polled from CTIX into MISP&quot;

initial_date_from = &quot;Enter the date from which you want to poll the data in the format YYYY-MM-DD&quot;

cron_seconds = 0 # Enter seconds to run this script as a cron job over this defined period.

misp_url = &quot;Enter your MISP URL&quot;

misp_api = &quot;Enter your MISP API key&quot;

- Perform the following operations using the commands. Use the help command for displaying help text. (python3 cron.py --help)

      Poll for Indicators: Polls for all **indicators** and upload them to MISP. Each indicator is parsed and displayed as an event in the MISP application.

      run for polling indicators: python3 cron.py --option poll_indicators

      Poll for Reports:** Polls for all report objects from CTIX and uploads them to MISP. Every report object is parsed and displayed as an event in the MISP application. In case the report object is associated with any indicators, the script extracts the indicators from these report objects and then uploads them to your MISP instance within a single event. All additional information contained in the report apart from indicators is converted into tags on the MISP event. This includes objects such as tags, campaigns, threat actors, etc.

      run for polling reports: python3 cron.py --option poll_reports

## Process Flow

CIC-MISP contains two integral sections

- TAXII Module - TAXII subscriber credentials from CTIX to export STIX data from CTIX
- MISP Rest API Module - MISP&#39;s REST API to ingest the data polled from CTIX into MISP.



1. This script implements these two modules as classes and polls for indicators from CTIX over the TAXII protocol.
2. It sends the indicators/ reports to MISP&#39;s REST API, which then proceeds to convert the STIX data into a MISP event.
3. You can now view CTIX indicators and report objects on MISP.

## Terminating Task
 showing cron job:  crontab -l
 terminating cron job: crontab -r
 kill the proccess: ps aux | grep cic-misp --> kill -9 {proccess_id}




Copyright (c) <2021> Cyware Labs, inc
