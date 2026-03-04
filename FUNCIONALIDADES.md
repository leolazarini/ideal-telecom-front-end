# Documentação de Funcionalidades — Ideal Telecom (POC)

> **Última atualização:** 2026-03-04 (v2)
> **Status do projeto:** Em desenvolvimento ativo

---

## Visão Geral

Sistema web de gestão para a **Ideal Telecom**, cobrindo o ciclo completo de relacionamento com clientes: contratos, pedidos, suporte técnico (chamados), gestão de usuários, logs de auditoria e relatórios.

---

## Stack Tecnológica

| Camada | Tecnologia |
|---|---|
| Linguagem | HTML5 + JavaScript Vanilla |
| Estilização | Tailwind CSS (CDN) |
| Ícones | Lucide Icons |
| Gráficos | ApexCharts |
| Fonte | Inter (Google Fonts) |
| Idioma | Português (pt-BR) |
| Build | Nenhum (arquivos HTML estáticos) |

**Paleta de cores (tema customizado Tailwind):**
- `brand-light`: `#1597D5` — Azul claro
- `brand-default`: `#19476B` — Azul marinho escuro
- `brand-teal`: `#0D9DA5` — Teal
- `brand-orange`: `#EB5435` — Laranja (alertas)
- `brand-yellow`: `#F5A62D` — Amarelo (avisos)
- `brand-pink`: `#E71552` — Rosa (erros)
- `brand-bg`: `#F8FAFC` — Fundo claro

---

## Estrutura de Arquivos (21 páginas HTML)

```
ideal-telecom-front-end/
├── index.html                  → Login
├── dashboard.html              → Painel principal
│
├── configuracoes-ofertas.html  → Gestão de Ofertas (novo)
│
├── clientes-lista.html         → Lista de clientes
├── cliente-novo.html           → Criar cliente (modal)
├── cliente-detalhes.html       → Detalhes do cliente
│
├── contratos-lista.html        → Lista de contratos
├── contrato-novo.html          → Criar contrato (modal)
├── contrato-detalhes.html      → Detalhes do contrato
│
├── pedidos-lista.html          → Lista de pedidos (kanban/tabela)
├── pedido-novo.html            → Criar pedido (modal)
├── pedido-detalhes.html        → Detalhes do pedido
│
├── chamados-lista.html         → Lista de chamados
├── chamado-novo.html           → Criar chamado (modal)
├── chamado-detalhes.html       → Detalhes do chamado
│
├── usuarios-lista.html         → Gestão de usuários
├── usuario-novo.html           → Criar usuário (modal)
│
├── logs-lista.html             → Logs de auditoria
│
├── relatorio-contratos.html    → Relatório de contratos
├── relatorio-pedidos.html      → Relatório de pedidos
└── relatorio-chamados.html     → Relatório de chamados

img/
├── logo-idealtelecom.svg
├── logo-hangar.svg
└── favicon-idealtelecom.svg
```

---

## Navegação (Sidebar Fixa)

```
Sidebar
├── Logo Ideal Telecom
├── Navegação principal
│   ├── Dashboard
│   ├── Clientes
│   ├── Contratos
│   ├── Pedidos
│   └── Chamados (badge com tickets críticos)
├── Administração
│   ├── Usuários
│   └── Logs do Sistema
├── Configurações  ← (novo módulo)
│   └── Ofertas
├── Relatórios
│   ├── Contratos
│   ├── Pedidos
│   └── Chamados
└── Rodapé do usuário
    ├── Avatar + Nome (Valter)
    ├── Cargo (Gestor)
    ├── Configurações (link para Ofertas)
    └── Logout
```

**Header (sticky):**
- Título da página
- Botões de ação rápida: Novo Cliente, Novo Pedido, Novo Contrato, Novo Chamado
- Sino de notificações (com badge)
- Avatar/dropdown de perfil

---

## Módulos e Funcionalidades

### 1. Autenticação (`index.html`)

- Formulário de login com e-mail e senha
- Checkbox "Lembrar-me"
- Link "Esqueci minha senha"
- Layout dividido: lado esquerdo (mensagem de marketing) / lado direito (formulário)
- Credenciais de demo: `valter@idealtelecom.com.br` / `12345678`

---

### 2. Dashboard (`dashboard.html`)

**KPIs exibidos (4 cards):**
| Card | Valor | Tendência |
|---|---|---|
| Contratos Ativos | 142 | +4,2% |
| Novos Pedidos | R$ 45,2k | +12% |
| Contratos vencendo em 30 dias | 8 | Alerta |
| Tickets Críticos | 2 | Risco |

**Seções da página:**
- Tabela de **Últimos Pedidos** (cliente, valor, status, link de ação)
- Tabela de **Próximos Vencimentos** (cliente, data de término, dias restantes)
- Gráfico de barras **Novos Pedidos** — últimos 6 meses (ApexCharts, valores em R$)
- Tabela de **Últimos Chamados** (ID, assunto, cliente, status, ação)

---

### 3. Gestão de Clientes

#### Lista (`clientes-lista.html`)
- Tabela com todos os clientes
- Busca e filtros
- Botão "Novo Cliente" → abre modal de criação
- Ações por linha: visualizar, editar, excluir

#### Criar Cliente (`cliente-novo.html`)
- Formulário de cadastro de cliente
- **Integração CNPJ (ReceitaWS):** ao informar o CNPJ e sair do campo, os dados da empresa são preenchidos automaticamente via API (Razão Social, Endereço, CEP, E-mail, Telefone)
- **Três pessoas separadas:**
  - **Representante Legal** — informações completas obrigatórias: nome, CPF, RG, data de nascimento, cargo, e-mail, telefone, endereço residencial
  - **Administrador do Contrato** — apenas dados de contato: nome, e-mail, telefone, WhatsApp
  - **Autorizado a Receber** — apenas dados de contato: nome, e-mail, telefone, WhatsApp

#### Detalhes (`cliente-detalhes.html`)
- Perfil completo do cliente
- Contratos e pedidos associados
- Histórico de chamados
- Card "Pessoas Vinculadas" exibindo: Representante Legal, Administrador do Contrato e Autorizado a Receber
- Botões de ação (editar, Transferir Carteira)

---

### 4. Gestão de Contratos

#### Lista (`contratos-lista.html`)
- Tabela: ID, Cliente, Valor, Status, Data de Término, Ações
- Filtros por status, período e cliente
- Badges coloridos de status

#### Criar Contrato (`contrato-novo.html`)
- Modal com formulário
- Campos: cliente, datas de início/término, valor, detalhes do serviço

#### Detalhes (`contrato-detalhes.html`)
- Informações completas do contrato
- Dados do cliente vinculado
- Timeline do contrato
- Pedidos e itens de serviço relacionados
- Informações de renovação/vencimento
- Botões de ação

---

### 5. Gestão de Pedidos

#### Lista (`pedidos-lista.html`)
- Visualização em **kanban** ou **tabela**
- Colunas/status: Novo → Negociação → Aprovado → Concluído → Cancelado
- Scrollbar customizado para kanban
- Filtros por status

#### Criar Pedido (`pedido-novo.html`)
- Seleção de cliente (modal de busca)
- **Itens do Pedido (estrutura dinâmica):** adicione N itens, cada um com:
  - Tipo de Produto (Móvel, Fixa, Internet, Consultoria, PBX em Nuvem)
  - Operadora (Claro, Vivo, TIM, Oi, Algar, Nextel)
  - Tipo de Negociação (filtrado por produto)
  - Oferta (filtrada por produto + operadora + negociação — vinculada ao módulo de Ofertas)
  - Valor do Item
  - Quantidade de Linhas
  - **Gerar Linhas:** ao informar Qtd > 0, botão "Gerar Linhas" cria campos individuais para informar os números telefônicos (opcional)
- **Valor Total:** somatório automático dos itens exibido ao final

#### Detalhes (`pedido-detalhes.html`)
- Dados do pedido e do cliente vinculado
- **Tabela de Itens** com: Oferta, Produto, Operadora, Negociação, Linhas cadastradas / total, Valor
- Valor total calculado
- Timeline / histórico do pedido
- **Botão "Gerar Contrato":** validação — se o pedido contém item do tipo **Móvel** com quantidade de linhas > 0 e nenhuma linha cadastrada, a geração é bloqueada com modal de alerta indicando quais itens precisam ter linhas informadas

---

### 6. Gestão de Chamados (Suporte)

#### Lista (`chamados-lista.html`)
- Tabela: ID, Assunto, Cliente, Status, Prioridade, Data, Ações
- Filtros por status: Aberto, Análise, Fechado
- Indicadores de severidade com cores
- Badge com contagem de tickets críticos (exibe "2" na sidebar)

#### Criar Chamado (`chamado-novo.html`)
- Modal com formulário
- Campos: cliente, assunto, descrição, categoria, prioridade, anexo

#### Detalhes (`chamado-detalhes.html`)
- Informações completas do ticket
- Thread de conversa/comentários com respostas da equipe
- **Campo SLA no tratamento:** ao registrar um atendimento, informa-se as horas de SLA daquele registro + checkbox "Considerar somente horas úteis" (não marcado por padrão)
- Modal de alteração de status com: Tipo do Chamado (lista padronizada) + Status do Atendimento
- Dados do cliente vinculado
- Indicadores de status e prioridade

#### Lista de Tipos de Chamado (padronizada)
`Ativação e desativação de serviço` · `Troca de chip` · `Cancelamento` · `Suspensão` · `Contestação de Faturas` · `Falha na Internet Móvel` · `Falha na Internet Fixa` · `Outros`

---

### 7. Gestão de Usuários (Administração)

#### Lista (`usuarios-lista.html`)
- Tabela: Nome, E-mail, Cargo, Status, Último Login
- Filtros por departamento/cargo
- Ações: editar, excluir, redefinir senha, desativar

#### Criar Usuário (`usuario-novo.html`)
- Modal com formulário
- Campos: nome, e-mail, senha, cargo, permissões, departamento
- Papéis: Admin, Gestor, Agente, Visualizador

---

### 8. Logs de Auditoria (`logs-lista.html`)

- Trilha de auditoria de todas as atividades do sistema
- Colunas: Timestamp, Usuário, Ação, Entidade, Status, Detalhes
- Filtros por: usuário, tipo de ação, período, entidade (Cliente/Contrato/Pedido)
- Busca e exportação de logs

---

### 9. Relatórios

#### Contratos (`relatorio-contratos.html`)
- Estatísticas e análises de contratos
- Filtros: período, cliente, status
- Gráficos: contratos por status, receita por período
- Métricas resumidas + exportação (PDF/Excel)

#### Pedidos (`relatorio-pedidos.html`)
- Estatísticas e análises de pedidos
- Filtros: período, status, cliente
- Gráficos: volume de pedidos, tendências de receita, distribuição por status
- Top clientes por pedidos + exportação

#### Chamados (`relatorio-chamados.html`)
- Estatísticas de tickets de suporte
- Filtros: período, status, prioridade
- Gráficos: tickets por status, métricas de tempo de resolução
- Métricas de desempenho por agente + exportação

---

## Fluxos Principais

### Login
```
index.html → preenche e-mail/senha → clica "Entrar no Sistema" → dashboard.html
```

### Criação de Cliente
```
clientes-lista.html → "Novo Cliente" → modal cliente-novo.html → submit → cliente-detalhes.html (ou retorno à lista)
```

### Ciclo de Pedido
```
pedidos-lista.html → "Novo Pedido" → modal pedido-novo.html → status "Novo"
→ Negociação → Aprovado → Concluído (kanban drag-and-drop ou troca manual)
```

### Ciclo de Chamado
```
chamados-lista.html → "Novo Chamado" → modal chamado-novo.html → status "Aberto"
→ Análise → Fechado (via chamado-detalhes.html)
```

### Relatório
```
Sidebar (seção Relatórios) → seleciona tipo → aplica filtros → visualiza gráficos → exporta
```

---

## Autenticação e Papéis

| Papel | Acesso |
|---|---|
| Admin | Acesso total ao sistema |
| Gestor | Funções de gestão |
| Agente | Funções operacionais/suporte |
| Visualizador | Somente leitura |

- Usuário logado exibido no rodapé da sidebar (nome + cargo)
- Controle de acesso aplicado no servidor (backend)
- Todas as ações são registradas nos logs de auditoria

---

## Componentes de UI Reutilizados

| Componente | Uso |
|---|---|
| KPI Cards | Dashboard — métricas com ícone, valor e tendência |
| Data Tables | Listas de todas as entidades |
| Modais | Formulários de criação/edição |
| Status Badges | Indicadores coloridos de status |
| Gráficos ApexCharts | Dashboard e relatórios |
| Search Bars | Filtros em todas as listas |
| Botões de ação (ícones Lucide) | Visualizar, editar, excluir, exportar |
| Grid responsivo | 1 col (mobile) → 4 col (desktop) |

---

## Endpoints de API Esperados

```
Autenticação
  POST /api/auth/login
  POST /api/auth/logout
  POST /api/auth/forgot-password

Clientes
  GET/POST   /api/clients
  GET/PUT/DELETE /api/clients/:id

Contratos
  GET/POST   /api/contracts
  GET/PUT/DELETE /api/contracts/:id
  GET        /api/contracts/expiring  (janela de 30 dias)

Pedidos
  GET/POST   /api/orders
  GET/PUT/DELETE /api/orders/:id
  PATCH      /api/orders/:id/status   (atualização kanban)

Chamados
  GET/POST   /api/tickets
  GET/PUT/DELETE /api/tickets/:id
  PATCH      /api/tickets/:id/status

Usuários
  GET/POST   /api/users
  GET/PUT/DELETE /api/users/:id

Logs
  GET        /api/logs
  POST       /api/logs  (automático a cada ação)

Relatórios
  GET        /api/reports/contracts
  GET        /api/reports/orders
  GET        /api/reports/tickets
  POST       /api/reports/export
```

---

---

### 10. Configurações — Ofertas (`configuracoes-ofertas.html`) ← **novo**

- Listagem de todas as ofertas cadastradas
- Filtros: busca por nome, produto, operadora, negociação
- **Cadastro de oferta vinculada por:** Produto + Operadora + Tipo de Negociação + Nome
- Campos adicionais: descrição, status (Ativa / Inativa)
- Edição e exclusão de ofertas (com confirmação)
- As ofertas cadastradas aqui alimentam o dropdown de Oferta nos Itens de Pedido

---

## Histórico de Alterações

| Data | Descrição |
|---|---|
| 2026-03-04 | **v2** — Itens de Pedido com linhas telefônicas; validação Gerar Contrato para Móvel sem linhas; módulo Configurações > Ofertas; Representante Legal + Administrador + Autorizado a Receber no cliente; integração CNPJ ReceitaWS; campo SLA em chamados com flag horas úteis; lista de tipos de chamado padronizada |
| — | `89c288c` — Ajustes gerais; modais nas telas de criação de cliente/pedido/contrato; tela de detalhes de contrato |
| — | `9d87f44` — Estrutura inicial das telas administrativa e de logs |
