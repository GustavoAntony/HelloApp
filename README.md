# AF CyberSecurity

## Nome dos Participantes

- **Erik Leonardo**
- **Gustavo Antony de Assis**
- **Matheus Aguiar**

---

## Projeto

### 1. **Descrição da Arquitetura do Sistema**

A arquitetura da rede é dividida em duas sub-redes:

- **Sub-rede pública**: Hospeda a API e as ferramentas de monitoramento **Zabbix** e **Wazuh**.
- **Sub-rede privada**: Contém o banco de dados, configurado para comunicação externa apenas por meio de um **NAT Gateway**, garantindo maior segurança ao isolar o banco de dados de acessos diretos da internet.

#### Fluxo de Tráfego

- **Cloudflare**: Desempenha um papel essencial na segurança e desempenho, redirecionando o tráfego com base na porta de destino:
  - **Porta 80 (HTTP)**: O tráfego é enviado diretamente para o **FastAPI**, sem passar pelo jumper.
  - **Outras portas**: O tráfego é encaminhado para o **jumper**, que funciona como um proxy centralizado. O **jumper** gerencia o roteamento para as aplicações internas, como **Zabbix** e **Wazuh**, além de atuar como um ponto de controle para otimizar e proteger os fluxos internos.

Essa arquitetura utiliza o **Cloudflare** para reforçar a segurança, oferecendo proteção contra ataques DDoS, enquanto o **jumper** proporciona flexibilidade no gerenciamento do tráfego interno.

#### Segurança do Banco de Dados

O **banco de dados** localizado na sub-rede privada só pode ser acessado pela **API** hospedada na sub-rede pública ou via **jump server**. Isso garante um ambiente mais seguro, isolando o banco de dados de acessos externos.

#### Ferramentas de Monitoramento

- **Zabbix**: Focado em monitorar a infraestrutura, com funcionalidades como disponibilidade, desempenho e análise de métricas de servidores e redes.
- **Wazuh**: Voltado para segurança, com foco em análise de logs, detecção de intrusão e conformidade com padrões regulatórios, como PCI DSS e GDPR.
- **Pritunl**: Utilizado para configurar o **jump server**, permitindo a conexão e gerenciamento dos usuários. Possibilita a autenticação com **MFA** para acessar os recursos internos da VPC.

Essa arquitetura, com a separação clara entre sub-redes, o uso estratégico do **jumper** e a proteção do **Cloudflare**, assegura tanto a observabilidade e desempenho da infraestrutura quanto a segurança do ambiente operacional.

---

### 2. **Diagrama de Arquitetura**

![Diagrama de arquitetura](./imgs/arq_img.jpeg)

---

### 3. **Rotas da api**

#### API URL

https://api.abcplace.art.br/docs

#### API`s Routes

##### Notes Application API Documentation

**Endpoint: /token**

Method: POST

- Request Body:
```json
{
  "username": "string",
  "password": "string"
}
```

- Response:
```json
{
  "access_token": "string",
  "token_type": "bearer"
}
```

**Endpoint: /notes/**

Method: POST

Authentication: Required (Bearer Token)

- Request Body:
```json
{
  "title": "string", 
  "text": "string"
}
```

**Endpoint: /notes/**

Method: GET

Authentication: Required (Bearer Token)

- Response: List of all notes

**Endpoint: /notes/{note_id}**

Method: GET

Authentication: Required (Bearer Token)

- Parameters:

note_id: Integer ID of the note

- Response: Specific note object

**Endpoint: /notes/{note_id}**

Method: DELETE

Authentication: Required (Bearer Token)

- Parameters:

note_id: Integer ID of the note

- Response:
```json
{
  "message": "Note deleted successfully"
}
```

**Endpoint: /**

Method: GET

- Response:
```json
{
  "message": "Hello World"
}
```
---

### 4. **Evidências de Testes de Segurança**

**Teste de acesso ao Zabbix sem vpn**
![Acesso ao Zabbix sem vpn](./imgs/zabbix-sem-vpn.jpeg)


**Teste de acesso ao Zabbix com vpn**
![Acesso ao Zabbix sem vpn](./imgs/zabbix-com-vpn.jpeg)

**Teste de acesso ao Wazuh com vpn**
![Acesso ao Wazuh sem vpn](./imgs/wazuh-com-vpn.jpeg)

**Teste de acesso ao Wazuh com vpn**

![Acesso ao Wazuh sem vpn](./imgs/wazuh-sem-vpn.jpeg)

**Teste de acesso a ec2 da api**

![Acesso sem vpn](./imgs/api_sem_vpn.png)

![Acesso com vpn](./imgs/api_com_vpn.png)

---

### 5. **Vídeo Demonstrativo do Ambiente em Funcionamento**

Confira o vídeo com uma demonstração do ambiente em funcionamento (até 7 minutos):

[![Vídeo demonstração](https://img.youtube.com/vi/0_wZRbrYUnc/0.jpg)](https://youtu.be/0_wZRbrYUnc)

---

### 6. **Alerta Configurado no Zabbix**

![Zabbix midia type](./imgs/zabbix-midia-type.jpeg)

---

### 7. **Teste do Alerta Feito via Zabbix**

![Zabbix alert](./imgs/zabbix-alert.jpeg)

---
