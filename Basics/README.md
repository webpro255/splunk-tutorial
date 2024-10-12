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
  ----

  ## New and Improved: Hidden Gems in Splunk
In this section, we'll cover some underutilized features that can significantly enhance your Splunk experience.

### Accelerated Data Models
- **What Are They?**
    - Data models that are accelerated allow for faster searches on large datasets by precomputing results.

- **Benefits**:
    - Improves performance of Pivot reports and dashboards.
    - Reduces search load on indexers.

- **How to Use**:
    - Define a data model in the Data Model Editor.
    - Enable acceleration by checking the **Accelerate** option and setting the **Summary Range**.

- **Best Practices**:
    - **Resource Planning**: Ensure your infrastructure can handle the additional storage and processing.
    - **Monitoring**: Use the **Monitoring Console** to keep an eye on data model accelerations.
  ---

  ### Using the Splunk Deployment Server
- Purpose:
    - Centralize the management of Splunk configurations and apps across multiple instances, simplifying administration.

- **Key Concepts**:
    - **Deployment Server**: The central Splunk instance that manages configurations.
    - **Deployment Clients**: Splunk instances that receive configurations from the deployment server.
    - **Server Classes**: Groups of deployment clients that share common configurations.
    - **Apps**: Packages containing configurations to be deployed.
- **Setup Steps:**
1. **Enable Deployment Server**:
    ```
    splunk enable deploy-server
    ```
2. **Create Server Classes**:
- Define server classes in `serverclass.conf` or via Splunk Web UI.
3. **Prepare Apps**:
- Place apps in `$SPLUNK_HOME/etc/deployment-apps/`.
4. **Configure Deployment Clients**:
- On each client, set up `deploymentclient.conf` to point to the deployment server.
    ```
    [target-broker:deploymentServer]
    targetUri = deploymentserver:8089
    ```
5. **Reload Deployment Server**:
    ```
    splunk reload deploy-server
    ```
- **Best Practices**:
    - Organize Server Classes Logically: Group clients based on role, location, or function.
    - Stagger Deployments: Schedule deployments during maintenance windows to minimize impact.
---

### Index-Time Field Extraction
- **What Is It?**
    - Extracting fields during indexing rather than at search time, storing them in the index for faster retrieval.

- **Advantages**:
    - Performance: Improves search speed as fields are pre-extracted.
    - Consistency: Ensures fields are extracted uniformly across all data.

- **Use Cases**:
    - Fields that are frequently searched and critical for reporting or alerting.

- **Implementation**:
1. **Define Transforms**:
    - In transforms.conf, define the field extraction using regular expressions.
    ```
    [extract_user]
    REGEX = user=(\w+)
    FORMAT = user::$1
    ```
2. **Configure Props**:
   - In `props.conf`, apply the transform at index time.
   ```
   [sourcetype]
   TRANSFORMS-index = extract_user
   ```
- **Caution**:
    - Irreversible: Index-time extractions cannot be modified after data is indexed.
    - Resource Intensive: May increase indexing time and storage requirements.

- **Best Practices**:
    - Plan Carefully: Only extract fields that are necessary and justify the overhead.
    - Test Thoroughly: Use a test environment to validate configurations before applying them to production.
---

## Additional Resources

- **Splunk Documentation**:
    - [Configuration File Reference](https://docs.splunk.com/Documentation/Splunk/latest/Admin/Aboutconfigurationfiles)
    - [Data Onboarding Manual](https://docs.splunk.com/Documentation/Splunk/latest/Data/Aboutaddingdata)
    - [Deployment Server Manual](https://docs.splunk.com/Documentation/Splunk/latest/Updating/Aboutdeploymentserver)

- **Splunk Blogs**:
    - [Best Practices for Data Onboarding](https://www.splunk.com/en_us/blog/tips-and-tricks.html?301=/en_us/category/tips-and-tricks)

- **Community Forums**:
    - [Splunk Answers](https://community.splunk.com/)
- **Users Groups**:
      - [Groups](https://usergroups.splunk.com/)
      
