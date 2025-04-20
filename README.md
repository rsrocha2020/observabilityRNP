# 📊 Zabbix + Grafana Monitoring Stack (Docker Compose)

Este projeto provisiona uma stack completa de monitoramento utilizando **Zabbix 7.2 LTS**, **MySQL**, **Grafana** e **Zabbix Java Gateway** com volume persistente em disco local. O deploy é realizado via docker compose.

Máquina virtualizada em Host Proxmox com 8G de memória, 2vCPUs, 1 Disco de 50G e partição utilizada no projeto /srv montada no servidor NFS. Sistema operacional Oracle Linux 8.

---

🎯 Objetivo

Fornecer uma solução de monitoramento confiável e modular, com instalação rápida via Docker Compose, utilizando boas práticas de separação de serviços, uso de variáveis de ambiente e persistência de dados.

---

## 🧹 Componentes da Stack

| Serviço               | Descrição                                                                 |
|-----------------------|---------------------------------------------------------------------------|
| **MySQL 8.0**         | Banco de dados utilizado pelo Zabbix Server                               |
| **Zabbix Server**     | Coleta e armazena os dados de monitoramento                               |
| **Zabbix Java Gateway** | Suporte ao monitoramento de aplicações Java (JMX)                        |
| **Zabbix Web (NGINX)**| Interface Web para administração do Zabbix                                |
| **Zabbix Agent**      | Agente para coleta de métricas no host/container                          |
| **Grafana OSS**       | Visualização gráfica com dashboards personalizados e plugin Zabbix        |

---

## 🗂️ Estrutura do Projeto

![image](https://github.com/user-attachments/assets/90bf15d8-bb06-4c09-845f-e3ec9d9a3185)

---

## ⚙️ Versão das aplicações:

- Docker `>= 20.10`
- Docker Compose `>= 1.29`
- SO: RHEL 8 ou compatível (podendo ser adaptado)
- Zabbix 7.2
- Grafana 11.6.0

---

## 🔐 Variáveis de Ambiente (`.env`)

Arquivo `.env` na raiz do projeto /srv/rnp:

# MySQL
MYSQL_DATABASE=zabbix

MYSQL_USER=zabbix

MYSQL_PASSWORD=(.env)

MYSQL_ROOT_PASSWORD=(.env)

# Zabbix Web
ZBX_WEB_ADMIN_USER=Admin

ZBX_WEB_ADMIN_PASSWORD=(.env)

# Grafana
GF_SECURITY_ADMIN_USER=admin

GF_SECURITY_ADMIN_PASSWORD=(.env)

---

## 🚀 Deploy

### 1. Clone o repositório

git clone https://github.com/rsrocha2020/observabilityRNP.git

### 2. Crie o diretório de volumes

sudo mkdir -p /srv/rnp/mysql_data /srv/rnp/grafana_data

sudo chown -R 1000:1000 /srv/rnp/mysql_data

sudo chown -R 472:472 /srv/rnp/grafana_data   # UID 472 para o user do Grafana

### 3. Suba os containers

docker-compose --env-file .env up -d

---

## 🖥️ Acesso aos Serviços

| Serviço       | URL                      | Login                         |
|---------------|--------------------------|-------------------------------|
| Zabbix Web    | http://localhost         | `Admin / (.env)`    |
| Grafana       | http://localhost:3000    | `admin / (.env)`   |

---

## 📦 Plugins e Extensões

- Grafana Plugin: `alexanderzobnin-zabbix-app` é instalado automaticamente mas precisa habilitar
- Monitoramento via JMX com Java Gateway ativado
- Persistência de dados MySQL e Grafana sob `/srv/rnp`

---

## 📌 Observações

- As senhas são gerenciadas via `.env`;
- O Zabbix Server conecta ao banco e inicializa os dados caso esteja vazio;
- Use volumes externos persistentes para backup/restauração.

---

## 🖥️ Observabilidade
- Ping - latência (rtt) e perda de pacotes(%):
  
![image](https://github.com/user-attachments/assets/f35d25bc-b312-458c-82f0-f3e39a84b4b3)

- Tempo de carregamento de páginas web e códigos de retorno (200, 404, etc)
  - google.com
  - youtube.com
  - rnp.br
  
![image](https://github.com/user-attachments/assets/569cd997-8f87-4128-9f55-6c060a82788d)

- Armazenar resultados em banco de dados (SQL ou No-SQL):
  
![image](https://github.com/user-attachments/assets/fbb892d6-6018-45e7-8bc4-73fda5f95f5b)

- Criar dashboards no Grafana para visualização":
  
![image](https://github.com/user-attachments/assets/00213530-d13b-4bac-8b28-515fd2a9771b)

---

## 👨‍💻 Autor

> Desenvolvido por https://github.com/rsrocha2020.


