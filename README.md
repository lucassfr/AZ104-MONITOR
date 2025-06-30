# AZ104-MONITOR

# Laboratório Prático: Monitoramento de Máquinas Virtuais no Azure

Este repositório foi criado como parte de um laboratório prático com foco na **configuração e gerenciamento de monitoramento de recursos no Microsoft Azure**, especialmente voltado para **máquinas virtuais (VMs)**.

O objetivo principal é:
- Aprender a manter **visibilidade**, **controle** e **resposta proativa** sobre eventos críticos no ambiente de nuvem.
- Demonstrar como configurar alertas, coletar logs e analisar métricas.
- Criar um repositório de **resumos, anotações e dicas úteis** para consulta futura e apoio nos estudos.

---

## Conceitos Fundamentais

### Azure Monitor
O **Azure Monitor** é uma plataforma centralizada para monitorar recursos do Azure. Ele permite:
- Coleta de métricas e logs
- Criação de alertas
- Visualização de desempenho
- Integração com ferramentas como Log Analytics e Application Insights

---

### Métricas vs Logs

| Tipo     | Descrição                          | Exemplo                                  |
|----------|------------------------------------|------------------------------------------|
| Métricas | Dados numéricos em tempo real      | CPU %, Disco IOPS, uso de memória        |
| Logs     | Dados estruturados e eventos       | Logs de atividade, eventos do sistema    |

---

##  Passo a Passo para Monitorar uma VM

### 1. Criar um **Log Analytics Workspace**
O workspace é onde os logs da VM serão armazenados e consultados com **Kusto Query Language (KQL)**.

- Navegue até **Log Analytics Workspaces**
- Clique em **Criar**
- Escolha **Grupo de Recursos**, **Região** e **Nome**

### 2. Habilitar Diagnóstico na VM
Você precisa **vincular a VM ao workspace** criado.

- Vá até a **máquina virtual**
- Clique em **Monitoramento > Logs**
- Selecione o workspace de destino
- Habilite a coleta de **logs e métricas**

### 3. Instalar o **Azure Monitor Agent (AMA)**
A nova geração de agente de monitoramento substitui o antigo MMA/OMS.

- Pode ser instalado automaticamente pelo portal ao associar a VM ao Log Analytics
- Também pode ser instalado manualmente ou via script/Policy

---

##  Criação de Regras de Alerta

### Componentes de uma Regra:
- **Recurso-alvo** (por exemplo, uma VM específica)
- **Condição** (ex: uso de CPU > 90%)
- **Ação** (envio de e-mail, chamada de webhook, execução de função)
- **Grupo de Ação** (onde são configurados os contatos e métodos de notificação)

### Exemplo de alerta:
> Notificar a equipe de TI por e-mail sempre que uma VM for **excluída**.

1. Vá em **Monitor > Alertas > Nova Regra**
2. Escolha o **Recurso**: assinatura ou grupo de recursos
3. Tipo de Sinal: **Registro de Atividade**
4. Condição: **Operação = "Excluir máquina virtual"**
5. Criar ou selecionar um **Grupo de Ação**
6. Definir **detalhes do alerta**

---

##  Consultas com KQL


Exemplo para listar todas as VMs com heartbeat ausente nos últimos 10 minutos:

```kusto
Heartbeat
| summarize LastSeen = max(TimeGenerated) by Computer
| where LastSeen < ago(10m)

 Dicas Úteis
O Azure Monitor Agent (AMA) é recomendado para novas implementações.

Use o IP Flow Verify do Network Watcher para checar se há bloqueio por NSG.

Ative o Soft Delete no Recovery Services Vault para evitar perda acidental de backup.

Use Dashboards do Azure Monitor para criar painéis visuais com métricas em tempo real.

Automatize alertas com Azure Logic Apps para resposta rápida e automatizada a eventos.

 Fontes de Estudo
Módulo Microsoft Learn: Configurar o monitoramento para máquinas virtuais

Documentação oficial do Azure Monitor

Referência KQL (Kusto Query Language)

 Resultado Esperado
Ao final deste laboratório, você terá:

Uma VM monitorada com logs e métricas centralizadas

Regras de alerta configuradas

Compreensão prática dos principais recursos do Azure Monitor

Este repositório como base para revisões e estudos futuros
