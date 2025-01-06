# Site24x7 APM Insight Java-Agent Ansible Role

## Table of Contents

1. [Overview](#overview)
2. [Requirements](#requirements)
3. [Configuration](#configuration)
    * [APM Insight Configuration Properties](#apm-insight-configuration-properties)
    * [Role Installation Properties](#role-installation-properties)
4. [Example Playbook and Inventory File](#example-playbook-and-inventory-file)
5. [Usage](#usage)
6. [License](#license)

## Overview
------------

Deploy and configure the Site24x7 APM Insight Java agent on your application servers with ease using this Ansible role. Supports popular application servers and frameworks like Apache Tomcat, Eclipse Jetty, WildFly, and Spring Boot.

## Requirements
---------------

* `unzip` command must be available on target hosts.
* Works only for operating systems `RedHat`, `Rocky`, `AlmaLinux`, `Debian`, `Darwin`.

## Configuration
-------------

### APM Insight Configuration Properties

| Property | Description | Required |
| --- | --- | --- |
| `app_name` | Application name for monitoring | No (auto-generated if omitted) |
| `license_key` | Site24x7 APM Insight License Key | **Yes** |

### Role Installation Properties

| Property | Description | Required | Default |
| --- | --- | --- | --- |
| `server_type` | Web server type (e.g., tomcat, jetty, wildfly) | **Yes** | - |
| `server_version` | Web server version | No | - |
| `jvm_config_file` | Path to web server's JVM config file | **Yes** | - |
| `service_name` | Service name for management | No | - |
| `restart_web_server` | Restart web server after installation | No | `true` |
| `agent_download_dir` | Directory for agent files (optional) | No | `/opt/apm` |
| `enable_agent` | Enable/disable the APM Insight agent | No | `true` |

## Example Playbook and Inventory File
--------------------------------------

### `apm_insight_playbook.yml`
```yml
- hosts: prod_webserver
  vars:
    apminsight_props:
      app_name: <Your Application Name>
      license_key: <Your License Key>
    install_props:
      server_type: 'tomcat'
      jvm_config_file: '/usr/share/tomcat8/bin/setenv.sh'
      service_name: 'tomcat'
      server_version: '8'
      restart_web_server: 'true'
  tasks:
   - include_role:
       name: Site24x7-APM
```    
### `host_inventory.yml`

```yml
 prod_webserver: # host group
  vars:
    # Here you can apply configuration to all hosts
    ansible_user: fedora
    server_type: tomcat
  hosts:
    java_server_one:
      ansible_host: 10.0.20.5
      #Host-specific agent configuration goes here
      java_agent_host_config:
        #Overrides app_name set in playbook
        app_name: Example App
    java_server_two:
      ansible_host: 127.0.40.2
      #app_name set in playbook will be used
```
## Usage
--------
Use the below command to extract the Ansible role and rename it to ```Site24x7-APM```

```curl -L -o APMInsightAnsible-main.zip https://github.com/mohasin-ali/APMInsightAnsible/archive/refs/heads/main.zip && unzip APMInsightAnsible-main.zip && mv APMInsightAnsible-main Site24x7-APM```

Write your own Playbook called my-playbook.yml and Inventory file called my-inventory.yml and keep the files in parallel to folder `Site24x7-APM`

After extracting the zip file with the above command, the final folder structure with the above two files (my-playbook.yml and my-inventory.yml) should look like below
```
├─ Site24x7-APM
├    ├── defaults
├    ├── handlers
├    ├── meta
├    ├── tasks
├    ├── tests
├    ├── vars
├    ├── README.md
├─ my-playbook.yml
├─ my-inventory.yml
```

Run the playbook with the below command

 ``` ansible-playbook -i ./my-inventory.yml ./my-playbook.yml ```

## License
--------
Copyright (c) 2023 Zoho Coroporation Pvt. ltd.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
