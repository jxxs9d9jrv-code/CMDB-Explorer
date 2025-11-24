# CMDB Explorer

Um conjunto completo de widgets ServiceNow para explorar e gerenciar dispositivos CMDB de forma visual e intuitiva.

## 🎯 Visão Geral

O CMDB Explorer oferece uma solução integrada para visualizar servidores, dispositivos de rede e suas configurações através de widgets no ServiceNow Portal.

### Funcionalidades Principais

- 📊 **Sidebar de Navegação** - Menu lateral dinâmico para alternar entre abas
- 🖥️ **Grid de Servidores** - Visualização de servidores por tipo com filtro de busca
- 🌐 **Grid de Redes** - Visualização de dispositivos de rede com filtro de busca
- ⬅️ **Botão Voltar Inteligente** - Retorna à página correta (Servers ou Networks)
- 💾 **Persistência de Preferências** - Salva a view escolhida pelo usuário

## 📁 Estrutura do Projeto

```
cmdb-explorer/
├── README.md
├── Pages/
│   │   ├── cmdb_portal_index/
│   │      ├── widgets
│   │      │── cmdb_pageview/
│   │      ├── cmdb_sidebar/
│   ├── widgets/
│   │   ├── cmdb_sidebar/
│   │   ├── cmdb_servers/
│   │   ├── cmdb_networks/
│   │   └── cmdb_back_button/
│   └── scripts/
│       └── includes/
│           └── global.PortalFilterPrefs.js
```

## 🚀 Como Usar

### 1. Widgets Disponíveis

#### **CMDB Sidebar**
Menu principal com navegação entre abas. Salva automaticamente qual abinha o usuário escolheu.

#### **CMDB Servers**
- Exibe todos os servidores ativos na CMDB
- Agrupa por tipo (Linux, Windows, ESX, etc)
- Filtro de busca em tempo real
- Clique em um servidor para ver detalhes

#### **CMDB Networks**
- Exibe todos os dispositivos de rede ativos
- Agrupa por tipo (Firewall, Switch, Router, etc)
- Filtro de busca em tempo real
- Clique em um dispositivo para ver detalhes

#### **CMDB Back Button** (em construcao)
- Botão dinâmico que volta para a página correta
- Se veio de Servers, volta para Servers
- Se veio de Networks, volta para Networks

## 🔧 Configuração

### URLs Base
Edite em cada controller se precisar mudar as URLs:

```javascript
var CONFIG = {
    detailsUrl: 'https://dev357601.service-now.com/sp?id=cmdb_details',
    ajaxClass: 'global.PortalFilterPrefs'
};
```

### Mapeamento de Servidores (Ainda nao dinamizado) - Escolhe a imagem baseado no sys_class_name
Em `cmdb_servers/server_script.js`:

```javascript
var SERVER_CLASS_MAPPING = {
    'cmdb_ci_linux_server': { name: 'Linux Server', image: 'linuxserver.png' },
    'cmdb_ci_win_server': { name: 'Windows Server', image: 'windowsserver.png' }
    // Adicione mais tipos conforme necessário
};
```

### Mapeamento de Redes (Ainda nao dinamizado)  - Escolhe a imagem baseado no sys_class_name
Em `cmdb_networks/server_script.js`:

```javascript
var NETWORK_CLASS_MAPPING = {
    'cmdb_ci_firewall': { name: 'Firewall', image: 'firewall.png' },
    'cmdb_ci_switch': { name: 'Switch', image: 'switch.png' }
    // Adicione mais tipos conforme necessário
};
```

## 📊 Fluxo de Dados

```
Sidebar (muda view)
    ↓
saveView() → salva preferência
    ↓
Carrega widget (Servers ou Networks)
    ↓
Usuário clica em um item
    ↓
saveFilter() → salva qual item foi selecionado
    ↓
Navega para cmdb_details
    ↓
Clica em "Voltar" (rever logica)
    ↓
getView() → lê preferência salva
    ↓
Volta para a página correta
```

## 🛠️ Requisitos

- ServiceNow Instance (Dublin ou superior)
- Acesso ao módulo de Portal
- Permissões para criar/editar widgets
- Script Include global.PortalFilterPrefs criado

## 📝 Script Include Necessário

Crie um novo Script Include chamado **`global.PortalFilterPrefs`** com o código em `src/scripts/includes/global.PortalFilterPrefs.js`

## ✨ Recursos Adicionais

### Filtro de Busca
- Busca em tempo real nos nomes dos itens
- Funciona em ambos os widgets (Servers e Networks)

### Contadores
- Mostra total de servidores/dispositivos ativos
- Mostra quantos tipos diferentes existem

### Estados de Carregamento
- Spinner animado durante carregamento
- Mensagens de erro apropriadas
- Estado vazio quando não há resultados

## 🐛 Solução de Problemas

### v1.0.0
- ✅ Primeiro release
- ✅ Sidebar dinâmica
- ✅ Widgets de Servers e Networks
- ✅ Botão voltar inteligente
- ✅ Persistência de preferências

## 👨‍💻 Desenvolvedor

Desenvolvido como solução de exploração CMDB no ServiceNow.

## 📄 Licença

Este projeto é fornecido como está para uso em ambientes ServiceNow.
