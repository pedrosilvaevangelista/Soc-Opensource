# SOC e Proteção Ativa Open Source

[![GitHub stars](https://img.shields.io/github/stars/pedrosilvaevangelista/Soc-Opensource.svg)](https://github.com/pedrosilvaevangelista/Soc-Opensource/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/pedrosilvaevangelista/Soc-Opensource.svg)](https://github.com/pedrosilvaevangelista/Soc-Opensource/network)

---

## 📋 1. Sobre o Projeto

Este projeto apresenta a **arquitetura e implementação** de um **Security Operations Center (SOC)** completo utilizando exclusivamente **ferramentas open source**. O objetivo é demonstrar como criar uma solução robusta de segurança cibernética com capacidades de **detecção, monitoramento, análise e resposta a incidentes** de forma integrada e funcional.

> **⚠️ Importante**: Este repositório tem como foco **apresentar a solução implementada** e sua arquitetura. Não inclui configurações específicas, scripts de instalação ou tutoriais passo-a-passo. Para implementação, consulte a documentação oficial de cada ferramenta.

### 1.1 🎯 Capacidades Implementadas

| Capacidade | Descrição |
|------------|-----------|
| **Detecção Proativa** | Identificação de ameaças e comportamentos suspeitos em tempo real |
| **Visualização Centralizada** | Dashboard unificado de eventos de segurança |
| **Resposta Estruturada** | Workflow organizado para resposta a incidentes |
| **Análise Forense** | Investigação detalhada de casos de segurança |
| **Threat Intelligence** | Compartilhamento e correlação de indicadores de ameaças |
| **Arquitetura Simples** | Solução de baixo custo e fácil manutenção |
| **Integração Completa** | Comunicação automática entre todas as ferramentas |

---

## 🏗️ 2. Arquitetura da Solução

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Workstations  │    │    Firewall      │    │   SOC Servers   │
│                 │    │   (OPNsense)     │    │                 │
│ • Wazuh Agent   │──▶│ • CrowdSec        │───▶│ • Wazuh SIEM   │
│ • Sysmon        │    │ • Suricata       │    │ • DFIR-IRIS     │
│                 │    │ • ClamAV         │    │ • MISP          │
│                 │    │ • Wazuh Agent    │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
```

### 2.1 📊 Visão Geral dos Componentes

| Camada | Componente | Função Principal |
|--------|-----------|------------------|
| **Endpoints** | Wazuh Agent | Coleta de logs e monitoramento local |
| **Endpoints** | Sysmon | Logging avançado de atividades do Windows |
| **Perímetro** | OPNsense | Firewall e roteamento de rede |
| **Perímetro** | CrowdSec | Detecção comportamental e bloqueio de IPs |
| **Perímetro** | Suricata | IDS/IPS com inspeção profunda de pacotes |
| **Perímetro** | ClamAV | Detecção de malware e vírus |
| **Core SOC** | Wazuh SIEM | Correlação de eventos e alertas |
| **Core SOC** | DFIR-IRIS | Gerenciamento de investigações forenses |
| **Core SOC** | MISP | Plataforma de threat intelligence |

---

## 🛡️ 3. Componentes Detalhados

### 3.1 🔥 Camada de Perímetro: Firewall/Roteador

#### OPNsense
**Firewall e roteador open source baseado em FreeBSD**

| Ferramenta | Tipo | Funcionalidades Principais |
|-----------|------|----------------------------|
| **CrowdSec** | IDR (Intrusion Detection & Response) | • Análise de logs em tempo real<br>• Detecção de brute force e port scanning<br>• Compartilhamento de threat intelligence<br>• Bloqueio automático de IPs maliciosos |
| **Suricata** | IDS/IPS + DPI | • Detecção baseada em regras (signatures)<br>• Suporte a regras personalizadas<br>• Inspeção de protocolos em tempo real<br>• Logging detalhado de eventos |
| **ClamAV** | Antivírus | • Detecção de malware, trojans e vírus<br>• Scanning de arquivos em tempo real<br>• Atualizações automáticas de assinaturas |
| **Wazuh Agent** | Monitor | • Monitoramento de integridade de arquivos<br>• Coleta de eventos do sistema<br>• Detecção de ameaças local |

### 3.2 💻 Camada de Endpoints: Workstations

| Agente | Plataforma | Capacidades |
|--------|-----------|-------------|
| **Wazuh Agent** | Windows/Linux/macOS | • Coleta de logs do sistema operacional<br>• Monitoramento de integridade de arquivos<br>• Detecção de rootkits e malware<br>• Monitoramento de registry (Windows)<br>• Compliance checking (PCI DSS, GDPR, HIPAA) |
| **Sysmon** | Windows | • Logging detalhado de atividades do sistema<br>• Rastreamento de processos e conexões de rede<br>• Monitoramento de mudanças em arquivos<br>• Eventos de criação de processos<br>• Detecção de técnicas de evasão |

### 3.3 🖥️ Camada Core: Servidores SOC

| Plataforma | Categoria | Funcionalidades |
|-----------|-----------|-----------------|
| **Wazuh SIEM/XDR** | Análise e Correlação | • Coleta e correlação de logs de todas as fontes<br>• Análise comportamental e detecção de anomalias<br>• Alertas em tempo real com diferentes níveis de severidade<br>• Dashboard interativo para visualização<br>• Compliance reporting automatizado<br>• API REST para integrações |
| **DFIR-IRIS** | Resposta a Incidentes | • Gerenciamento de casos de incidentes<br>• Workflow de investigação estruturado<br>• Colaboração entre analistas<br>• Timeline de eventos detalhada<br>• Documentação de evidências<br>• Relatórios de incidentes automatizados<br>• Integração com Wazuh via webhook |
| **MISP** | Threat Intelligence | • Threat Intelligence centralizada<br>• Compartilhamento de IOCs (Indicators of Compromise)<br>• Correlação de ameaças entre casos<br>• Feed de inteligência externa<br>• API para automação<br>• Taxonomias e tags organizacionais |

---

## 📦 4. Requisitos de Infraestrutura

### 4.1 💾 Hardware Mínimo Recomendado

#### Firewall/Roteador

| Componente | Especificação Mínima | Observações |
|-----------|---------------------|-------------|
| **CPU** | 2 cores, 2.0 GHz | Processamento de regras e inspeção |
| **RAM** | 4 GB | Para operação estável |
| **Storage** | 32 GB SSD | Armazenamento de logs locais |
| **Network** | 2x NICs Gigabit | WAN e LAN |

#### Servidor SOC

| Componente | Especificação Mínima | Recomendado | Observações |
|-----------|---------------------|-------------|-------------|
| **CPU** | 4 cores, 2.5 GHz | 8 cores, 3.0 GHz | Para correlação de eventos |
| **RAM** | 16 GB | 32 GB | Elasticsearch consome bastante RAM |
| **Storage** | 500 GB SSD | 1 TB SSD | Retenção de logs históricos |
| **Network** | 1x NIC Gigabit | 1x NIC Gigabit | Comunicação com endpoints |

### 4.2 💿 Software Base

| Categoria | Requisito | Versões Suportadas |
|-----------|-----------|-------------------|
| **Sistema Operacional** | Linux | Ubuntu 20.04+, CentOS 8+, Debian 11+ |
| **Sistema Operacional** | FreeBSD | Para OPNsense |
| **Containerização** | Docker & Docker Compose | Versão estável mais recente |
| **Linguagem** | Python | 3.8+ |
| **Controle de Versão** | Git | Versão estável mais recente |

---

## 🔗 5. Ferramentas e Recursos Oficiais

### 5.1 📚 Links das Ferramentas

| Categoria | Ferramenta | Website Oficial | Descrição |
|-----------|-----------|----------------|-----------|
| **Firewall** | OPNsense | [opnsense.org](https://opnsense.org/) | Firewall e roteador baseado em FreeBSD |
| **Detecção** | CrowdSec | [crowdsec.net](https://www.crowdsec.net/) | Sistema de detecção colaborativo |
| **IDS/IPS** | Suricata | [suricata.io](https://suricata.io/) | Engine de inspeção profunda de pacotes |
| **Antivírus** | ClamAV | [clamav.net](https://www.clamav.net/) | Antivírus open source |
| **SIEM/XDR** | Wazuh | [wazuh.com](https://wazuh.com/) | Plataforma de SIEM e XDR |
| **Logging** | Sysmon | [Microsoft Docs](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon) | Logging avançado para Windows |
| **DFIR** | DFIR-IRIS | [dfir-iris.org](https://dfir-iris.org/) | Gerenciamento de investigações forenses |
| **TI Platform** | MISP | [misp-project.org](https://www.misp-project.org/) | Compartilhamento de threat intelligence |

---

## 📊 6. Fluxos Operacionais

### 6.1 🔄 Fluxo de Detecção e Resposta

```
① Evento Suspeito
    ↓
② Detecção (Suricata/CrowdSec)
    ↓
③ Alerta (Wazuh)
    ↓
④ Criação de Caso (DFIR-IRIS)
    ↓
⑤ Export de IOCs (MISP)
```

### 6.2 🔗 Arquitetura de Integração

```
Wazuh SIEM ──→ DFIR-IRIS (Criação automática de casos)
     ↓
DFIR-IRIS ──→ MISP (Export de IOCs após investigação)
     ↓
MISP ──→ Threat Intelligence (Compartilhamento de indicadores)
```

### 6.3 🛠️ Métodos de Comunicação

| Método | Uso | Finalidade |
|--------|-----|-----------|
| **Webhooks** | Wazuh → DFIR-IRIS | Automação de workflows |
| **APIs REST** | Todas as ferramentas | Integração de dados |
| **Feeds de TI** | MISP → Wazuh | Enriquecimento de alertas |
| **Dashboards** | Wazuh | Visualização unificada |

---

## 🎯 7. Capacidades Demonstradas

### 7.1 📈 Funcionalidades Principais

| Funcionalidade | Descrição |
|---------------|-----------|
| **Security Events Overview** | Dashboard centralizado com visão holística |
| **Compliance Monitoring** | Monitoramento de conformidade regulatória |
| **Threat Hunting** | Caça a ameaças proativa e investigativa |
| **Vulnerability Assessment** | Avaliação contínua de vulnerabilidades |
| **File Integrity Monitoring** | Monitoramento de integridade de arquivos críticos |

### 7.2 🔍 Workflows de Resposta Implementados

| # | Workflow | Cenário | Ferramentas Envolvidas |
|---|---------|---------|----------------------|
| **①** | Investigação de Malware | Detecção e análise de arquivos maliciosos | Wazuh + ClamAV + DFIR-IRIS + MISP |
| **②** | Resposta a Phishing | Investigação de campanhas de phishing | Wazuh + DFIR-IRIS + MISP |
| **③** | Resposta a Vazamento de Dados | Investigação de exfiltração de informações | Wazuh + Suricata + DFIR-IRIS |
| **④** | Investigação de Ameaças Internas | Análise de comportamento suspeito de usuários | Wazuh + Sysmon + DFIR-IRIS |

---

## 📈 8. Resultados e Métricas

### 8.1 🎯 Métricas de Eficiência

| Métrica | Sigla | Descrição |
|---------|-------|-----------|
| **Mean Time to Detection** | MTTD | Tempo médio entre o início de um ataque e sua detecção |
| **Mean Time to Response** | MTTR | Tempo médio entre a detecção e a resposta efetiva |
| **Taxa de Falsos Positivos** | FPR | Precisão e acurácia dos alertas gerados |
| **Cobertura de Endpoints** | - | Percentual de dispositivos monitorados |
| **Status dos Feeds de TI** | - | Qualidade e atualização da threat intelligence |

### 8.2 🎭 Tipos de Ameaças Detectadas

| # | Categoria | Exemplos |
|---|-----------|----------|
| **①** | Malware | Trojans, ransomware, spyware |
| **②** | Ataques de Força Bruta | Tentativas de login não autorizado |
| **③** | Exfiltração de Dados | Transferências suspeitas de arquivos |
| **④** | Integridade de Arquivos | Alterações não autorizadas em arquivos críticos |
| **⑤** | Command & Control | Comunicação com servidores C&C conhecidos |
| **⑥** | Insider Threats | Atividades suspeitas de usuários internos |

### 8.3 ✅ Benefícios da Implementação

| # | Benefício | Impacto |
|---|-----------|---------|
| **①** | Visibilidade Completa | Monitoramento de 100% da rede e endpoints |
| **②** | Resposta Coordenada | Workflow estruturado para tratamento de incidentes |
| **③** | Documentação Estruturada | Registro detalhado de todos os casos |
| **④** | Compartilhamento de Inteligência | Colaboração com a comunidade global |
| **⑤** | Custo Zero de Licenciamento | Solução totalmente open source |

---

## 🖼️ 9. Topologia Implementada

<h3>Exemplo Topologia Pós-implementação</h3>

<img src="assets/TOPOLOGIA DEPOIS.png" alt="Banner do Curso CiberLivre" width="1000"/>

---
## 🔗 10. Links Uteis

| Titulo | Link | Descrição |
|-----------|-----------|----------------|
| **Wazuh - Segurança Cibernética** | [Playlist](https://youtube.com/playlist?list=PLALjBisXuYJc15J1wWbxXwBLZcCSCjmUJ&si=0z-fGhPMOQkJFofg) | Configurações completas do sistema wazuh |
| **Instalação e configuração misp** | [video](https://youtu.be/xqP_l-AF4Jc) | Configuração basica misp |

## 📝 11. Notas Finais

**Se você utilizar este projeto em trabalhos acadêmicos ou implementações empresariais, por favor, mantenha os devidos créditos.**

**Desenvolvido com ❤️ para a comunidade de segurança cibernética**