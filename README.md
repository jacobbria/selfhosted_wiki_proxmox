# Self Hosting Wiki.js using Proxmox
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

        %% External Access Node
        Laptop[fa:fa-laptop Personal Laptop]

        %% The Physical Host (Surface Pro)
        subgraph Surface [fa:fa-server Surface Pro / Proxmox Host]
            style Surface fill:,stroke:#333,stroke-width:4px
            
            %% The Virtualized Containers
            Wiki[fa:fa-globe Wiki.js Container<br/>]
            DB[fa:fa-database Postgres Container<br/>]
            
            %% Internal Connection
            Wiki <-->|SQL Queries| DB
        end

        %% Connection from Laptop to Wiki
        Laptop --- Wiki
    end


