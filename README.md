# Promox Self-Host
Converting existing hardware into a Proxmox server to host a LAN available Wiki.js instance and PostgresSQL DB using containers and networking basics.

# Stack

# Stack

| **Virtualization** | **Infrastructure** | **Application** | **Database** | **Monitoring** | **Security** | **Versioning** |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| <img src="https://github.com/user-attachments/assets/baddab33-4de3-4d8e-89d3-dbb6ac7c608a" width="80" alt="Proxmox VE" /> | <img src="https://github.com/user-attachments/assets/009b9ae2-67a3-4771-997c-1f2a82312999" width="80" alt="Debian Linux" /> | <img src="https://github.com/user-attachments/assets/ad954a51-9b46-4b05-ba45-ea44594403b7" width="80" alt="Wiki.js" /> | <img src="https://github.com/user-attachments/assets/430f541f-84b3-48cf-a3eb-0f414df2ec6f" width="80" alt="PostgreSQL" /> | <img width="200" height="200" alt="zabbix" src="https://github.com/user-attachments/assets/f1e3680e-4c01-4d87-99b0-9b4846a10618" /> | <img width="225" height="225" alt="images" src="https://github.com/user-attachments/assets/c39cce57-ed35-4fa1-b6ff-566710ff3d4d" />   | <img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" width="80" alt="GitHub" /> |
| **Proxmox VE** | **Debian/LXC** | **Wiki.js** | **PostgreSQL** | **Zabbix** | **Wazuh** | **GitHub** |





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
            
            Wiki[fa:fa-globe Wiki.js]
            DB[fa:fa-database Postgres]
            
            Wiki --- DB
        end

        %% Node 2: Surface Pro
        subgraph Surface [fa:fa-tablet Surface Pro / Node 2]
            direction TB
            style Surface fill:none,stroke:#333,stroke-width:3px
            
            Zabbix[fa:fa-chart-line Zabbix Monitoring]
            Wazuh[fa:fa-shield-halved Wazuh SIEM]
        end

        %% Connections (The Management Plane flow)
        Laptop -.-> Zabbix
        Laptop -.-> Wiki
        
        %% The Monitoring Flow (The "Eyes" watching the "Worker")
        Zabbix --- Dell
        Wazuh --- Dell
    end
```


