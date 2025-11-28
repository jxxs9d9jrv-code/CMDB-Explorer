# Bradesco Seguros - CMDB Explorer

Um conjunto completo de widgets/pages para explorar e gerenciar dispositivos CMDB de forma visual e intuitiva.

##  Visão Geral

O CMDB Explorer oferece uma solução integrada para visualizar servidores, dispositivos de rede e suas configurações através de widgets no ServiceNow Portal.

### Funcionalidades Principais

- **Sidebar de Navegação** - Menu lateral dinâmico para alternar entre abas
- **Grid de Servers** - Visualização de servidores agrupados pelo (sys_class_name) com filtro de busca (via cliente ng-model)
- **Grid de Network** - Visualização de network agrupados pelo (sys_class_name) com filtro de busca (via cliente ng-model)
- **Persistência de Preferências** - Adinciana a preferencia do filtro no sys_preference do usuário em um Script Include

## Estrutura do Projeto

```
Bradesco Seguros: cmdb-explorer/
├── Pages/
    │   ├── cmdb_portal_index/
    │      ├── widgets
    │      │── cmdb_pageview/
    │      ├── cmdb_sidebar/
    │   ├── cmdb_servers_page/
    │      ├── widgets
    │      │── cmdb_servers_widget/
    │   ├── cmdb_network/
    │      ├── widgets
    │      │── cmdb_network/
    │   ├── cmdb_details/
    │      ├── widgets
    │      │── cmdb_data_table_from_url/
    └── scripts/
        └── includes/
            └── global.PortalFilterPrefs.js
```

## Como Usar

### 1. Widgets Disponíveis

#### **Name: CMDB Sidebar ID: cmdb_sidebar**
É o menu de navegação principal do CMDB Explorer. Funciona como a barra lateral fixa que permite alternar entre diferentes visualizações (Servidores e Dispositivos de Rede). É o ponto central de controle da aplicação, salvando automaticamente qual tabela iremos usar na busca na preferência do usuário.

#### **CMDB Servers**
Exibe todos os servidores ativos na CMDB agrupados por sys_classname + company. Oferece uma visualização em grid com cards informativos que mostram a quantidade de servidores de cada tipo, permitindo filtro de busca em tempo real e navegação para detalhes específicos.

#### **CMDB Networks**
Exibe todos as redes ativas na CMDB agrupados por sys_classname + company. Oferece uma visualização em grid com cards informativos que mostram a quantidade de redes de cada tipo, permitindo filtro de busca em tempo real e navegação para detalhes específicos.


#### **CMDB Page View**
Uma visualizacao de paginas que esta diretamente ligada ao cmdb_sidebar, ela atualiza sempre que o usuario escolher uma nova opcao do menu ou interagir com o que esta sendo mostrado por ela. 


#### **CMDB Data Table From URL**
Clone direto do widget Data Table From URL nele modificamos o filtro para interagir diretamente com o script include global.PortalFilterPrefs.js para mostrar os resultados da pesqueisa com base na preferencia do usuario 

## 🔧 Configuração

### URLs Base
Edite em cada controller se precisar mudar as URLs:

```javascript
function getBaseUrl() {
        var protocol = window.location.protocol;
        var host = window.location.host;
        return protocol + '//' + host;
    }

    // Configuração com URLs dinâmicas
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

### Mapeamento de Imagens (Ainda nao dinamizado) - Escolhe a imagem baseado no sys_class_name
Em `cmdb_servers/server_script.js & cmdb_network/cmdb_network.js`: Escolhemos a imagem com base no nome dela. 

```javascript
var SERVER_CLASS_MAPPING = {
    'cmdb_ci_aix_server': { name: 'AIX Server', image: 'aixserver.png', construcao: true },
    'cmdb_ci_esx_server': { name: 'ESX Server', image: 'aixserver.png' },
    'cmdb_ci_hcx_server': { name: 'HCX Server', image: 'aixserver.png' },
    'cmdb_ci_hpux_server': { name: 'HPUX Server', image: 'aixserver.png', construcao: true },
    'cmdb_ci_hyperv_server': { name: 'Hyper-V Servers', image: 'aixserver.png' },
    'cmdb_ci_linux_server': { name: 'Linux Server', image: 'linuxserver.png' },
    'cmdb_ci_server': { name: 'Generic Server', image: 'genericservers.png' },
    'cmdb_ci_solaris_server': { name: 'Solaris Server', image: 'aixserver.png', construcao: true },
    'cmdb_ci_vmware_vcenter': { name: 'VMware Vcenter', image: 'aixserver.png' },
    'cmdb_ci_win_server': { name: 'Windows Server', image: 'windowsserver.png' }
};

};
```

## Fluxo de Dados

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
```

## Requisitos

- ServiceNow Instance
- Itil
- Script Include global.PortalFilterPrefs

## Script Include Necessário
O script include global.PortalFilterPrefs é responsável por gerenciar as preferências do usuário no portal CMDB Explorer. Fornece métodos para salvar e recuperar as escolhas do usuário (view ativa, filtros aplicados, tabela selecionada).

## Recursos Adicionais

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
