# AjDashBoard: dashboard executivo do departamento jurídico

Painel web que transforma a planilha exportada do sistema de gestão jurídica em indicadores visuais para a coordenação: produtividade por responsável, prazos fatais, audiências e fila de atividades.

**Demo:** https://aj-dash-board.vercel.app (requer planilha configurada)

## Problema

A coordenação acompanhava a fila de tarefas em uma planilha extensa, sem visão consolidada. O painel lê essa planilha direto do SharePoint e mostra, em uma tela, o que vence hoje, quem está sobrecarregado e o que está atrasado.

## Funcionalidades

- **Indicadores de resumo** (cards) da fila de atividades
- **Aba Produtividade:** ranking de responsáveis e gráficos por área do direito e por status (Chart.js)
- **Aba Prazos:** painel de prazos fatais agrupados por responsável, com situação calculada (atrasado, vence hoje, amanhã ou nos próximos 7 dias)
- **Aba Audiências:** próximas audiências agrupadas por responsável
- **Aba Atividades:** filtro por responsável e por pendentes ou concluídas
- **Extração de prazo fatal de texto livre** via expressão regular (ex.: "FATAL: 24/09"), já que o sistema de origem não tem campo de data próprio para isso
- **Cache em localStorage** com botão de atualização e exibição da data da última extração
- **Tema claro e escuro** com detecção automática da preferência do sistema
- **Mensagens de erro orientativas** (ex.: link do SharePoint sem permissão), com classe de erro própria

## Arquitetura

JavaScript puro com **ES Modules**, organizado em **MVC**:

```
js/
├── model/        # regras de negócio e dados
│   ├── state.js          # estado da aplicação
│   ├── excelService.js   # download, validação e cache da planilha
│   ├── domain.js         # regras sobre cada registro (status, prazo fatal)
│   ├── aggregations.js   # rankings, agrupamentos e resumos
│   └── dateUtils.js      # conversão de datas do Excel
├── view/         # renderização (cards, gráficos, tabelas, abas, tema, modal)
└── controller/
    └── app.js    # orquestra carregamento e eventos
```

## Stack

JavaScript (ES6+ Modules), HTML5, CSS3 (variáveis de tema), Chart.js + chartjs-plugin-datalabels, SheetJS (xlsx), localStorage, deploy na Vercel.

## Como rodar localmente

```bash
git clone https://github.com/wesleybalb/AjDashBoard.git
cd AjDashBoard
```

Crie um arquivo `env.js` na raiz (ele está no `.gitignore` e não vai para o repositório):

```js
window.ENV = { EXCEL_URL: "<link de download da planilha>" };
```

Depois sirva a pasta por HTTP:

```bash
npx serve .
```

## Segurança

- O link da planilha fica em `env.js`, fora do versionamento
- Nenhum dado de processo é armazenado em servidor; o cache fica só no navegador de quem acessa

## Autor

Wesley Balbino · [LinkedIn](https://www.linkedin.com/in/wesley-balbino) · [GitHub](https://github.com/wesleybalb)
