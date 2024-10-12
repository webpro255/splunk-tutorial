# Basics

This section covers the fundamentals of Splunk, including installation guides, key concepts, and best practices for data onboarding and system configuration. It's designed to provide both newcomers and experienced users with valuable insights to get the most out of Splunk.

## Table of Contents

- [Introduction to Splunk](#introduction-to-splunk)
- [Installation Guide](#installation-guide)
  - [System Requirements](#system-requirements)
  - [Installation Steps](#installation-steps)
- [Understanding Splunk Architecture](#understanding-splunk-architecture)
  - [Components of Splunk](#components-of-splunk)
  - [Data Flow in Splunk](#data-flow-in-splunk)
- [Data Inputs and Parsing](#data-inputs-and-parsing)
  - [Data Onboarding Best Practices](#data-onboarding-best-practices)
  - [Advanced Data Parsing Techniques](#advanced-data-parsing-techniques)
- [Splunk Configuration Management](#splunk-configuration-management)
  - [Understanding Configuration Files](#understanding-configuration-files)
  - [Best Practices for Configuration Management](#best-practices-for-configuration-management)
- [**New and Improved: Hidden Gems in Splunk**](#new-and-improved-hidden-gems-in-splunk)
  - [Accelerated Data Models](#accelerated-data-models)
  - [Using the Splunk Deployment Server](#using-the-splunk-deployment-server)
  - [Index-Time Field Extraction](#index-time-field-extraction)
- [Additional Resources](#additional-resources)

---

## Introduction to Splunk

Splunk is a powerful platform for collecting, indexing, and analyzing machine-generated data from various sources in real-time. It enables organizations to gain insights from their data for operational intelligence, security, and more.

**Key Features:**

- **Data Ingestion:** Collect data from various sources, including logs, metrics, and event data.
- **Search and Investigation:** Use the Search Processing Language (SPL) to query and analyze data.
- **Visualization:** Create dashboards and reports for data visualization.
- **Alerts and Monitoring:** Set up alerts to monitor systems proactively.
- **Apps and Integrations:** Extend Splunk's functionality with apps and add-ons.

---

## Installation Guide

### System Requirements

Before installing Splunk, ensure your system meets the minimum requirements:

- **Operating Systems:** Windows, macOS, Linux (various distributions)
- **CPU:** 64-bit processors
- **Memory:** Minimum 4GB RAM (8GB recommended)
- **Disk Space:** At least 20GB free disk space

### Installation Steps

#### For Linux:

1. **Download Splunk Enterprise:**

   ```bash
   wget -O splunk-latest-linux-x86_64.tgz 'https://www.splunk.com/page/download_track?file=7.3.1/linux/splunk-7.3.1-f3e41e4b37b2-Linux-x86_64.tgz&ac=&wget=true&name=wget&platform=Linux&architecture=x86_64&version=7.3.1&product=splunk&typed=release'
   ```
2. **Extract the Package:**
   ```bash
   tar xvzf splunk-latest-linux-x86_64.tgz -C /opt
   ```
3. **Accept the License Agreement:**
   ```
   /opt/splunk/bin/splunk start --accept-license
   ```
4. **Set Admin Credentials:**
  - During the first start, you'll be prompted to create admin credentials.

#### For Windows:
1. **Download the Installer:**
  - Visit the [Splunk Download Page](https://www.splunk.com/en_us/download.html) and download the Windows installer.
2. **Run the Installer:**
  - Double-click the .msi file and follow the installation wizard.
3. **Accept License and Configure:**
  - Accept the license agreement and configure installation settings.
4. **Set Admin Credentials:**
  - During installation, set the admin username and password.
---

## Understanding Splunk Architecture

### Components of Splunk
- **Forwarders**: Collect and forward data to indexers.
- **Indexers**: Receive and index data from forwarders; handle search requests.
- **Search Heads**: Provide the user interface for searching, analyzing, and visualizing data.
- **Deployment Server**: Manages configurations for distributed Splunk environments.

### Data Flow in Splunk
1. **Data Input**: Data is ingested through forwarders or directly into indexers.
2. **Parsing**: Data is parsed, and metadata is added (e.g., timestamps, host, source).
3. **Indexing**: Parsed data is stored in indexes for efficient search and retrieval.
4. **Searching**: Users run searches via the search head to analyze data.

## Data Inputs and Parsing

### Data Onboarding Best Practices
- **Understand Your Data**:
  - **Source Identification**: Know where your data is coming from and what information it contains.
  - **Data Format**: Be aware of the data formats (e.g., JSON, CSV, XML) to handle them appropriately.

- **Use Forwarders**:
  - **Universal Forwarder**: Lightweight agent installed on source systems to collect and forward data.
  - **Heavy Forwarder**: Capable of parsing and indexing data before forwarding.

- **Define Input Configurations**:
   - Use `inputs.conf` to specify data inputs, including file paths, network ports, and modular inputs.

- Set Sourcetypes Correctly:
  - Assign appropriate `sourcetype` to categorize data, which aids in parsing and searching.

  ### Advanced Data Parsing Techniques
  - **Timestamp Recognition**:
      - Configure `TIME_FORMAT` and `TIME_PREFIX` in `props.conf` to recognize custom timestamp formats.
  - **Line Breaking**:
      - Use `LINE_BREAKER` and `SHOULD_LINEMERGE` in `props.conf` to define how events are separated.
  - **Character Encoding**:
      - Set `CHARSET` in `props.conf` if data is in a non-UTF-8 encoding to ensure correct parsing.
  - **Anonymization**:
      - Use `SEDCMD` in `props.conf` to mask sensitive data at index time, enhancing data security.
  ---

  ## Splunk Configuration Management
### Understanding Configuration Files
Splunk uses various configuration files located in `$SPLUNK_HOME/etc/`:

- **inputs.conf**: Defines data inputs.
- **props.conf**: Sets data parsing rules.
- **transforms.conf**: Defines data transformations and routing.
- **outputs.conf**: Configures forwarding destinations.
- **indexes.conf**: Defines index settings, such as storage paths and retention policies.

### Best Practices for Configuration Management

- **Layered Configuration**:
    - Understand the precedence between system (`/etc/system/`), app (`/etc/apps/`), and user (`/etc/users/`) configurations.
- **Use Apps for Configuration**:
    - Package configurations within apps for modularity and portability, making them easier to manage and deploy.
- **Version Control**:
    - Keep configuration files under version control systems like Git to track changes and collaborate effectively.
- **Backup Configurations**:
    - Regularly back up configuration files to prevent data loss and facilitate recovery.
