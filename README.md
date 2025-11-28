# Bradesco Seguros - CMDB Explorer

> **Portal CMDB completo** desenvolvido em ServiceNow, com navegação dinâmica, widgets customizados e persistência de preferências do usuário.

<div align="center">
  <img src="https://img.shields.io/badge/ServiceNow-CMDB-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Portal-Widgets-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-Production-orange?style=for-the-badge" />
</div>

---

## 📘 Sumário

* [Visão Geral](#-visão-geral)
* [Funcionalidades](#-funcionalidades)
* [Estrutura do Projeto](#-estrutura-do-projeto)
* [Widgets](#-widgets)
* [Configuração](#-configuração)
* [Fluxo de Dados](#-fluxo-de-dados)
* [Script Include Necessário](#-script-include-necessário)
* [Recursos Adicionais](#-recursos-adicionais)
* [Conclusão](#-conclusão)

---

## 🎯 Visão Geral

O **CMDB Explorer** oferece uma experiência moderna, rápida e integrada para explorar Servidores, Redes e Detalhes de Itens da CMDB usando o ServiceNow Portal.

Ele inclui:

* Navegação dinâmica
* Grids agrupados
* Preferências persistidas do usuário
* Widgets customizados conectados entre si

---

## ✨ Funcionalidades

* 📊 **Sidebar de Navegação** — troca entre páginas e salva a preferência automaticamente.
* 🖥️ **Grid de Servers** — agrupamento por `sys_class_name` + busca instantânea.
* 🌐 **Grid de Network Devices** — mesmo comportamento do módulo de servidores.
* 💾 **Persistência de Preferências** — salva views, filtros e navegações em `sys_user_preference`.
* 🔄 **Navegação Inteligente** — página de detalhes carregada com filtros aplicados dinamicamente.

---

## 📁 Estrutura do Projeto

```bash
Bradesco Seguros: cmdb-explorer/
├── Pages/
│   ├── cmdb_portal_index/
│   │   ├── widgets/cmdb_pageview/
│   │   └── cmdb_sidebar/
│   ├── cmdb_servers_page/
│   │   └── widgets/cmdb_servers_widget/
│   ├── cmdb_network/
│   │   └── widgets/cmdb_network/
│   ├── cmdb_details/
│   │   └── widgets/cmdb_data_table_from_url/
└── scripts/
    └── includes/global.PortalFilterPrefs.js
```

---

## 🧩 Widgets

### 📌 **CMDB Sidebar (ID: cmdb_sidebar)**

* Menu lateral fixo
* Atualiza a view principal
* Salva a opção selecionada no `sys_user_preference`

### 🖥️ **CMDB Servers**

* Agrupa servidores por `sys_class_name + company`
* Cards com totais e ícones
* Filtro rápido com `ng-model`

### 🌐 **CMDB Networks**

* Mesma experiência do módulo de servidores
* Focado em dispositivos de rede

### 🗂️ **CMDB Page View**

* Carrega dinamicamente a página ativa
* Sincroniza com a Sidebar e preferências

### 📄 **CMDB Data Table From URL (Custom)**

* Baseado no widget padrão
* Filtro reescrito para integrar automaticamente com `PortalFilterPrefs.js`

---

## 🔧 Configuração

### 🔗 URLs Dinâmicas por Ambiente

```javascript
function getBaseUrl() {
  return window.location.protocol + '//' + window.location.host;
}

var VIEW_CONFIG = {
  servers: {
    title: 'Servers Management',
    icon: 'fa-server',
    url: getBaseUrl() + '/brad_bsra?id=cmdb_servers_page'
  },
  network: {
    title: 'Network Devices',
    icon: 'fa-sitemap',
    url: getBaseUrl() + '/brad_bsra?id=cmdb_network'
  },
  server_details: {
    title: 'Server Details',
    icon: 'fa-list'
  }
};
```

### 🖼️ Mapeamento de Imagens por Classe

```javascript
var SERVER_CLASS_MAPPING = {
  'cmdb_ci_linux_server': { name: 'Linux Server', image: 'linuxserver.png' },
  'cmdb_ci_win_server': { name: 'Windows Server', image: 'windowsserver.png' },
  'cmdb_ci_server': { name: 'Generic Server', image: 'genericservers.png' },
  // Demais classes...
};
```

---

## 📊 Fluxo de Dados

```mermaid
flowchart TD
  A[Usuário escolhe view na Sidebar] --> B[saveView()]
  B --> C[Carrega Servers ou Networks]
  C --> D[Usuário clica em um item]
  D --> E[saveFilter()]
  E --> F[Redireciona para cmdb_details]
  F --> G[Data Table aplica filtro salvo]
```

---

## 📝 Script Include Necessário

### `global.PortalFilterPrefs.js`

Responsável por:

* Armazenar filtros e views em `sys_user_preference`
* Recuperar preferências ao carregar o portal
* Integrar filtros com widgets como *Data Table From URL*

---

## ✨ Recursos Adicionais

* 🔍 **Busca em tempo real** em Servers/Networks
* 🔢 **Contadores automáticos** por classe
* ⏳ **Estado de carregamento inteligente**
* ❌ **Fallback quando nenhum resultado é encontrado**

---

## 📌 Conclusão

O **CMDB Explorer** do Bradesco Seguros é uma solução completa, modular e escalável para visualização da CMDB no ServiceNow.
Com widgets integrados, preferências persistentes e navegação fluida, oferece uma experiência profissional e eficiente para times de Infraestrutura e ITSM.

---

Se quiser, posso adicionar:

* Badges extras
* GIF demonstrativo
* Documentação de API
* Capturas de tela do portal
* Versão em inglês
