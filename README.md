# Mini SOC — Monitoramento de Segurança com Wazuh

## 1. Visão Geral

Este projeto consiste na implementação de um laboratório básico de **Security Operations Center (SOC)** utilizando Wazuh, Rocky Linux e Windows 11.

O objetivo é desenvolver conhecimentos práticos em monitoramento de eventos de segurança, análise de logs, triagem de alertas e documentação de incidentes.

O ambiente simula atividades comuns de um analista SOC N1, incluindo identificação de eventos suspeitos, investigação de alertas e registro de evidências.

## 2. Objetivos

- Implementar e configurar o Wazuh Manager.
- Integrar um endpoint Windows à infraestrutura de monitoramento.
- Monitorar eventos de segurança dos sistemas Windows e Linux.
- Gerar eventos controlados para testar mecanismos de detecção.
- Investigar alertas utilizando o Wazuh.
- Documentar ocorrências por meio de relatórios estruturados de investigação.

## 3. Tecnologias Utilizadas

| Tecnologia | Finalidade |
|---|---|
| Rocky Linux | Servidor de monitoramento de segurança |
| Windows 11 | Endpoint monitorado |
| Wazuh Manager | Análise de eventos e geração de alertas |
| Wazuh Agent | Coleta de eventos do Windows |
| VirtualBox | Virtualização e configuração de redes |
| PowerShell | Administração e análise de logs do Windows |
| Linux CLI | Administração do Wazuh e investigação de alertas |

## 4. Arquitetura do Laboratório

```text
Windows 11 (Host)
    |
    | Wazuh Agent
    | Windows Event Logs
    |
    | Rede Host-only
    |
    v
Rocky Linux (VM)
    |
    +-- Wazuh Manager
    |
    +-- Monitoramento local do Linux
    |
    +-- Análise de alertas
    |
    +-- Investigação de eventos
```

### Configuração de Rede

O ambiente utiliza dois adaptadores de rede no VirtualBox:

- **NAT:** Permite o acesso da máquina virtual à internet.
- **Host-only:** Estabelece a comunicação entre o Windows e o servidor Wazuh instalado no Rocky Linux.

### Arquitetura do Wazuh

Foi adotada uma instalação simplificada, utilizando somente o Wazuh Manager, sem os componentes Indexer e Dashboard.

Essa abordagem reduz o consumo de recursos computacionais e permite realizar o monitoramento e a investigação de eventos diretamente pelo terminal Linux.

## 5. Implementação

### 5.1 Instalação do Wazuh Manager

O Wazuh Manager foi instalado no Rocky Linux utilizando o repositório oficial RPM e o gerenciador de pacotes DNF.

O Manager é responsável por:

- Receber dados dos endpoints monitorados.
- Processar eventos de segurança.
- Aplicar regras de detecção.
- Gerar alertas de segurança.
- Armazenar localmente os alertas identificados.

### 5.2 Instalação do Agente Windows

O Wazuh Agent foi instalado no sistema Windows e configurado para estabelecer comunicação com o Manager pela rede Host-only.

O agente permite coletar eventos de segurança do Windows e encaminhá-los ao servidor para análise.

### 5.3 Validação da Conectividade

A comunicação entre o endpoint e o servidor foi verificada por meio do comando:

```bash
sudo /var/ossec/bin/agent_control -l
```

O endpoint Windows foi registrado com sucesso e apresentou o status `Active`.

## 6. Cenários de Monitoramento

O laboratório contempla simulações controladas para exercitar procedimentos básicos de investigação de segurança.

### Cenário 1 — Falha de Autenticação no Windows

**Fonte:** Windows Security Event Log

**Event ID:** 4625

**Objetivo:** Identificar e analisar tentativas malsucedidas de autenticação no Windows.

Etapas previstas:

1. Gerar um evento controlado de falha de autenticação.
2. Verificar o registro no Visualizador de Eventos do Windows.
3. Identificar o alerta correspondente no Wazuh, caso seja gerado.
4. Analisar informações como horário, usuário, tipo de logon e motivo da falha.
5. Documentar as evidências e a classificação da ocorrência.

**Status:** Aguardando conclusão da investigação.

### Cenário 2 — Monitoramento de Autenticação no Linux

**Fonte:** Logs de autenticação do Rocky Linux

**Objetivo:** Monitorar e investigar falhas de autenticação no ambiente Linux.

Etapas previstas:

1. Gerar um evento controlado de autenticação malsucedida.
2. Localizar o registro nos logs do sistema.
3. Verificar se o Wazuh identificou e classificou o evento.
4. Analisar os detalhes e possíveis implicações de segurança.
5. Documentar a investigação.

**Status:** Aguardando implementação.

## 7. Análise de Alertas

Os alertas gerados pelo Wazuh podem ser consultados diretamente no terminal Linux:

```bash
sudo tail -f /var/ossec/logs/alerts/alerts.json
```

O procedimento de investigação adotado segue as etapas:

**Detecção → Triagem → Análise → Classificação → Documentação → Encerramento**

Cada investigação será registrada individualmente na pasta `investigations/`.

## 8. Limitações do Projeto

- O ambiente possui finalidade educacional.
- A implementação não inclui Wazuh Indexer ou Dashboard.
- A análise dos alertas é realizada por linha de comando.
- Os eventos simulados não representam necessariamente atividades maliciosas reais.
- Não foram implementados mecanismos automatizados de resposta a incidentes.

## 9. Referências

- [Documentação Oficial do Wazuh](https://documentation.wazuh.com/current/)
- [Regras de Detecção do Wazuh](https://documentation.wazuh.com/current/user-manual/ruleset/)
- [Auditoria de Segurança do Windows — Microsoft](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/)

## 10. Status do Projeto

**Em desenvolvimento**

- [x] Configuração da máquina virtual Rocky Linux
- [x] Instalação do Wazuh Manager
- [x] Configuração da comunicação de rede
- [x] Instalação do Wazuh Agent no Windows
- [x] Validação da conectividade entre agente e servidor
- [ ] Investigação de eventos de autenticação no Windows
- [ ] Investigação de eventos de autenticação no Linux
- [ ] Documentação das evidências coletadas
- [ ] Finalização da documentação
