# Site24x7 APM Insight Java Agent Ansible Role

## Table of Contents

1. [Overview](#overview)
2. [Requirements](#requirements)
3. [Configuration](#configuration)
    * [APM Insight Configuration Properties](#apm-insight-configuration-properties)
    * [Role Installation Properties](#role-installation-properties)
4. [Example Playbook and Inventory File](#example-playbook-and-inventory-file)
5. [Usage](#usage)
6. [Uninstallation](#uninstallation)
7. [License](#license)

## Overview
--------

Deploy and configure the Site24x7 APM Insight Java agent on your application servers with ease using this Ansible role. Supports popular application servers and frameworks like Apache Tomcat, Eclipse Jetty and WildFly.

## Requirements
--------

* The `unzip` command must be available on target hosts.
* Works only for operating systems `RedHat`, `Rocky`, `AlmaLinux`, `Debian` and `Darwin`.
* Ansible should be installed on the control node. [Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

## Configuration
--------

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
| `jvm_config_file` | Path to the web server's JVM config file. For eg. `setenv.sh` file in Tomcat server | **Yes** | - |
| `service_name` | Service name for management | No | - |
| `restart_web_server` | Restart the web server after installation | No | `true` |
| `agent_download_dir` | Directory for agent files (optional) | No | `/opt/apm` |
| `enable_agent` | Enable/disable the APM Insight agent | No | `true` |

## Example Playbook and Inventory File
---------

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
    # Here, you can apply the configuration to all hosts
    ansible_user: fedora
    server_type: tomcat
  hosts:
    java_server_one:
      ansible_host: 10.0.20.5
      #Host-specific agent configuration goes here
      java_agent_host_config:
        #Overrides app_name set in the playbook
        app_name: Example App
    java_server_two:
      ansible_host: 127.0.40.2
      #app_name set in playbook will be used
```
## Usage
--------
Use the below command to extract the Ansible role & rename it to ```Site24x7-APM```

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

 ## Uninstallation
--------
To uninstall the Site24x7 APM Insight Java Agent using Ansible, you can use the following variable property in your Ansible playbook:

| Property | Description | Required | Default |
| --- | --- | --- | --- |
| `uninstall` | Set to `true` to uninstall the agent | **Yes** | - |

### Example Ansible Playbook

Here is an example Ansible playbook that uninstalls the Site24x7 APM Insight Java Agent:

```yml
- hosts: webservers
  vars:
    uninstall: true
  tasks:
   - include_role:
       name: Site24x7-APM
```
This will initiate the uninstallation process for the Site24x7 APM Insight Java Agent.

### Important Notes

- Make sure to set the uninstall variable to true to uninstall the agent. If you set it to false or omit it, the agent will not be uninstalled.
- Replace webservers with the name of your host group in your Ansible inventory file.
- Make sure to include the Site24x7-APM role in your playbook to uninstall the agent.

## License
--------
(The MIT License)

Copyright © 2015, 2016 Site24x7 Terms of Use (http://www.site24x7.com/terms.html) Privacy Policy (http://www.site24x7.com/privacypolicy.html) Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NON-INFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
