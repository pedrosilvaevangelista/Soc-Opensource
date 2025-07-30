# SOC e Proteção Ativa Open Source

[![GitHub stars](https://img.shields.io/github/stars/pedrosilvaevangelista/Soc-Opensource.svg)](https://github.com/pedrosilvaevangelista/Soc-Opensource/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/pedrosilvaevangelista/Soc-Opensource.svg)](https://github.com/pedrosilvaevangelista/Soc-Opensource/network)

## 📋 Sobre o Projeto

Este projeto apresenta a **arquitetura e implementação** de um **Security Operations Center (SOC)** completo utilizando exclusivamente **ferramentas open source**. O objetivo é demonstrar como criar uma solução robusta de segurança cibernética com capacidades de **detecção, monitoramento, análise e resposta a incidentes** de forma integrada e funcional.

> **⚠️ Importante**: Este repositório tem como foco **apresentar a solução implementada** e sua arquitetura. Não inclui configurações específicas, scripts de instalação ou tutoriais passo-a-passo. Para implementação, consulte a documentação oficial de cada ferramenta.

### 🎯 O que foi Implementado

- **Detecção proativa** de ameaças e comportamentos suspeitos
- **Visualização centralizada** de eventos de segurança
- **Resposta estruturada** a incidentes
- **Análise forense** e investigação de casos
- **Threat Intelligence** compartilhada
- **Arquitetura simples** e de baixo custo
- **Integração completa** entre todas as ferramentas

## 🏗️ Arquitetura da Solução

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Workstations  │    │    Firewall      │    │   SOC Servers   │
│                 │    │   (OPNsense)     │    │                 │
│ • Wazuh Agent   │───▶│ • CrowdSec      │───▶│ • Wazuh SIEM    │
│ • Sysmon        │    │ • Suricata       │    │ • DFIR-IRIS     │
│                 │    │ • ClamAV         │    │ • MISP          │
│                 │    │ • Wazuh Agent    │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
```

## 🛡️ Componentes da Solução

### 🔥 Firewall/Roteador (OPNsense)

**OPNsense** - Firewall e roteador open source baseado em FreeBSD

#### Ferramentas Integradas:

- **🛡️ CrowdSec**: Sistema de detecção e resposta a intrusões (IDR) baseado em comportamento
  - Análise de logs em tempo real
  - Detecção de comportamentos maliciosos (brute force, port scanning, etc.)
  - Compartilhamento de threat intelligence com a comunidade
  - Bloqueio automático de IPs maliciosos

- **🔍 Suricata**: IDS/IPS e engine de inspeção profunda de pacotes (DPI)
  - Detecção baseada em regras (signatures)
  - Suporte a regras personalizadas
  - Inspeção de protocolos em tempo real
  - Logging detalhado de eventos

- **🦠 ClamAV**: Antivírus open source
  - Detecção de malware, trojans e vírus
  - Scanning de arquivos em tempo real
  - Atualizações automáticas de assinaturas

- **📊 Wazuh Agent**: Coleta de logs e monitoramento
  - Monitoramento de integridade de arquivos
  - Coleta de eventos do sistema
  - Detecção de ameaças local

### 💻 Workstations

#### Agentes de Monitoramento:

- **📊 Wazuh Agent**: 
  - Coleta de logs do sistema operacional
  - Monitoramento de integridade de arquivos
  - Detecção de rootkits e malware
  - Monitoramento de registry (Windows)
  - Compliance checking (PCI DSS, GDPR, HIPAA)

- **🔬 Sysmon**: Ferramenta avançada de logging do Windows
  - Logging detalhado de atividades do sistema
  - Rastreamento de processos e conexões de rede
  - Monitoramento de mudanças em arquivos
  - Eventos de criação de processos
  - Detecção de técnicas de evasão

### 🖥️ Servidores SOC

#### 📈 Wazuh SIEM/XDR
- **Coleta e correlação** de logs de todas as fontes
- **Análise comportamental** e detecção de anomalias
- **Alertas em tempo real** com diferentes níveis de severidade
- **Dashboard interativo** para visualização
- **Compliance reporting** automatizado
- **API REST** para integrações

#### 🔍 DFIR-IRIS (Digital Forensics and Incident Response)
- **Gerenciamento de casos** de incidentes
- **Workflow de investigação** estruturado
- **Colaboração entre analistas**
- **Timeline de eventos** detalhada
- **Documentação de evidências**
- **Relatórios de incidentes** automatizados
- **Integração com Wazuh** via webhook

#### 🌐 MISP (Malware Information Sharing Platform)
- **Threat Intelligence** centralizada
- **Compartilhamento de IOCs** (Indicators of Compromise)
- **Correlação de ameaças** entre casos
- **Feed de inteligência** externa
- **API para automação**
- **Taxonomias e tags** organizacionais

## 📦 Pré-requisitos

### Hardware Mínimo Recomendado

#### Firewall/Roteador:
- **CPU**: 2 cores, 2.0 GHz
- **RAM**: 4 GB
- **Storage**: 32 GB SSD
- **Network**: 2x NICs Gigabit

#### Servidor SOC:
- **CPU**: 4 cores, 2.5 GHz
- **RAM**: 16 GB (32 GB recomendado)
- **Storage**: 500 GB SSD (1 TB recomendado)
- **Network**: 1x NIC Gigabit

### Software:
- **Sistema Operacional**: Linux (Ubuntu 20.04+, CentOS 8+, Debian 11+) / e FreeBSD
- **Docker & Docker Compose** (para alguns componentes)
- **Python 3.8+**
- **Git**

## 🔗 Ferramentas e Links Oficiais

### 🔥 Firewall/Roteador
- **[OPNsense](https://opnsense.org/)** - Firewall e roteador open source baseado em FreeBSD

### 🛡️ Detecção e Proteção
- **[CrowdSec](https://www.crowdsec.net/)** - Sistema de detecção e resposta a intrusões colaborativo
- **[Suricata](https://suricata.io/)** - IDS/IPS e engine de inspeção profunda de pacotes
- **[ClamAV](https://www.clamav.net/)** - Antivírus open source

### 📊 SIEM e Monitoramento
- **[Wazuh](https://wazuh.com/)** - Plataforma open source de SIEM e XDR
- **[Sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon)** - Ferramenta de logging avançado para Windows

### 🔍 Resposta a Incidentes
- **[DFIR-IRIS](https://dfir-iris.org/)** - Plataforma web para gerenciamento de investigações forenses
- **[MISP](https://www.misp-project.org/)** - Plataforma de compartilhamento de threat intelligence

## 📊 Exemplo da Solução Implementada

### Fluxo de Detecção e Resposta:
```
1. Evento Suspeito → 2. Detecção (Suricata/CrowdSec) → 3. Alerta (Wazuh) → 4. Caso (DFIR-IRIS) → 5. IOCs (MISP)
```

### Capacidades Demonstradas:
- **Security Events Overview** - Dashboard centralizado
- **Compliance Monitoring** - Monitoramento de conformidade
- **Threat Hunting** - Caça a ameaças proativa
- **Vulnerability Assessment** - Avaliação de vulnerabilidades
- **File Integrity Monitoring** - Monitoramento de integridade

### Workflows de Resposta:
- **Investigação de Malware**
- **Resposta a Phishing**
- **Resposta a Vazamento de Dados**
- **Investigação de Ameaças Internas**

## 🔧 Arquitetura de Integração

### Fluxo de Dados Implementado:
```
Wazuh SIEM ──→ DFIR-IRIS (Criação automática de casos)
     ↓
DFIR-IRIS ──→ MISP (Export de IOCs após investigação)
     ↓
MISP ──→ Threat Intelligence (Compartilhamento de indicadores)
```

### Comunicação Entre Ferramentas:
- **Webhooks** para automação de workflows
- **APIs REST** para integração de dados
- **Feeds de Threat Intelligence** para enriquecimento
- **Dashboards unificados** para visualização

## 📈 Resultados Obtidos

### Métricas de Eficiência:
- **MTTD** (Mean Time to Detection) - Tempo médio de detecção
- **MTTR** (Mean Time to Response) - Tempo médio de resposta
- **Taxa de Falsos Positivos** - Precisão dos alertas
- **Cobertura de Endpoints** - Abrangência do monitoramento
- **Status dos Feeds de TI** - Qualidade da threat intelligence

### Tipos de Ameaças Detectadas:
- Malware e trojans
- Tentativas de força bruta
- Exfiltração de dados
- Alterações não autorizadas em arquivos
- Comunicação com servidores C&C
- Atividades suspeitas de usuários

### Benefícios da Implementação:
- **Visibilidade completa** da rede e endpoints
- **Resposta coordenada** a incidentes
- **Documentação estruturada** de casos
- **Compartilhamento de inteligência** de ameaças
- **Custo zero** de licenciamento

## 🤝 Contribuição

Contribuições são bem-vindas!

## 🙏 Créditos e Agradecimentos

Este projeto foi desenvolvido com foco em democratizar o acesso a soluções de segurança cibernética de alta qualidade.

**Se você utilizar este projeto em trabalhos acadêmicos ou implementações empresariais, por favor, mantenha os devidos créditos.**

## 📞 Suporte

Para dúvidas, sugestões ou suporte:

- 📧 **Email**: [pedrosilvaevangelista2005@gmail.com]
- 💬 **Issues**: [GitHub Issues](https://github.com/pedrosilvaevangelista/Soc-Opensource/issues)

---

⭐ **Se este projeto foi útil para você, considere dar uma estrela!**

**Desenvolvido com ❤️ para a comunidade de segurança cibernética**
