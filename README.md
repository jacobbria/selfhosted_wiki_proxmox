# Promox Self-Host
Converting existing hardware into a Proxmox server to host a LAN available Wiki.js instance and PostgresSQL DB using containers and networking basics.

# Stack

| **Virtualization** | **Infrastructure** | **Application** | **Database** | **Versioning** |
| :---: | :---: | :---: | :---: | :---: |
| <img src="https://github.com/user-attachments/assets/baddab33-4de3-4d8e-89d3-dbb6ac7c608a" width="100" alt="Proxmox VE" /> | <img src="https://github.com/user-attachments/assets/009b9ae2-67a3-4771-997c-1f2a82312999" width="100" alt="Debian Linux" /> | <img src="https://github.com/user-attachments/assets/ad954a51-9b46-4b05-ba45-ea44594403b7" width="100" alt="Wiki.js" /> | <img src="https://github.com/user-attachments/assets/430f541f-84b3-48cf-a3eb-0f414df2ec6f" width="100" alt="PostgreSQL" /> | <img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" width="100" alt="GitHub" /> |
| **Proxmox VE** | **Debian/LXC** | **Wiki.js** | **PostgreSQL** | **GitHub** |


# Architecture

```mermaid
graph TD
    %% Define the LAN Boundary
    subgraph LAN [fa:fa-wifi Home LAN]
        direction TB
        style LAN fill:none,stroke:#999,stroke-width:2px,stroke-dasharray: 5 5

        %% Central Access Point
        Laptop[fa:fa-laptop Personal Laptop]

        %% Node 1: Dell Server
        subgraph Dell [fa:fa-server Dell Server / Node 1]
            direction TB
            style Dell fill:none,stroke:#0078d7,stroke-width:3px
            
            Wiki[fa:fa-globe Wiki.js&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]
            DB[fa:fa-database Postgres&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]
            
            Wiki --- DB
        end

        %% Node 2: Surface Pro
        subgraph Surface [fa:fa-tablet Surface Pro / Node 2]
            direction TB
            style Surface fill:none,stroke:#333,stroke-width:3px
            
            Zabbix[fa:fa-chart-line Zabbix Monitoring]
            Wazuh[fa:fa-shield-halved Wazuh SIEM&nbsp;&nbsp;&nbsp;]
            
            Zabbix --- Wazuh
        end

        %% Connections
        Laptop --- Dell
        Laptop --- Surface
        Dell --- Surface
    end
```


