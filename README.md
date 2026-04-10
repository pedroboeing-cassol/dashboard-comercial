# ECOMM Performance Dashboard

Dashboard de performance por categoria para campanhas Google Ads com identificador ECOMM.

---

## 📁 Estrutura do projeto

```
ecomm-dashboard/
├── index.html              ← Dashboard (GitHub Pages)
├── google-ads-script.js    ← Script do Google Ads
└── README.md               ← Este arquivo
```

---

## 🚀 Passo a Passo — Configuração Completa

### Etapa 1 — Criar a Planilha Google Sheets

1. Acesse [sheets.google.com](https://sheets.google.com) e crie uma planilha nova
2. Anote o **ID da planilha** (parte da URL entre `/d/` e `/edit`)
   - Exemplo: `https://docs.google.com/spreadsheets/d/**1BxiMVs0XRA5...**`
3. Clique em **Compartilhar** → **Qualquer pessoa com o link** → **Leitor**
   - ⚠️ Isso é obrigatório para o dashboard conseguir ler os dados

---

### Etapa 2 — Configurar o Script no Google Ads

1. Acesse [ads.google.com](https://ads.google.com)
2. Menu → **Ferramentas** → **Scripts**
3. Clique em **+** para criar novo script
4. Cole todo o conteúdo de `google-ads-script.js`
5. Na linha `SPREADSHEET_ID`, substitua pelo ID da sua planilha:
   ```js
   SPREADSHEET_ID: "1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgVE2upms",
   ```
6. Clique em **Autorizar** (permissão para acessar o Sheets)
7. Clique em **Executar** para testar — verifique os logs
8. Vá em **Agendamentos** → **+ Novo** → Diário → Horário: 07:00

---

### Etapa 3 — Publicar o Dashboard no GitHub Pages

1. Crie uma conta em [github.com](https://github.com) (se ainda não tiver)
2. Clique em **New repository** → Nome: `ecomm-dashboard` → Public → Create
3. Faça upload dos arquivos:
   - Opção A (interface web): arraste `index.html` e `README.md` para o repositório
   - Opção B (Git):
     ```bash
     git init
     git add .
     git commit -m "Dashboard inicial"
     git remote add origin https://github.com/SEU_USUARIO/ecomm-dashboard.git
     git push -u origin main
     ```
4. Acesse **Settings** → **Pages** → Source: **Deploy from branch** → Branch: `main` → `/root`
5. Aguarde ~1 min. Seu dashboard estará em:
   `https://SEU_USUARIO.github.io/ecomm-dashboard/`

---

### Etapa 4 — Configurar o ID no Dashboard

Abra `index.html` e localize a linha:

```js
const SHEET_ID = "COLE_AQUI_O_ID_DA_PLANILHA";
```

Substitua pelo ID da sua planilha e faça o commit novamente.

---

## 🔧 Mapeamento de Colunas da Planilha

| Coluna | Conteúdo |
|--------|----------|
| A | Data |
| B | Campanha |
| C | ID do Item |
| D | Título |
| E | Categoria (L1) |
| F | Subcategoria (L2) |
| G | Tipo Produto L3 |
| H | Tipo Produto L4 |
| I | Tipo Produto L5 |
| J | Custo (R$) |
| K | Impressões |
| L | CPM |
| M | Cliques |
| N | CTR (%) |
| O | CPC (R$) |
| P | Conversões |
| Q | Custo por Conversão (R$) |
| R | Valor de Conversão (R$) |
| S | ROAS |
| T | Taxa de Conversão (%) |

---

## ⚙️ Configurações do Script

```js
var CONFIG = {
  SPREADSHEET_ID: "...",      // ID da planilha
  SHEET_NAME: "dados_brutos", // Nome da aba
  DAYS_TO_FETCH: 15,          // Janela de dias
  CAMPAIGN_KEYWORDS: ["ecomm", "[ecomm]"]  // Filtros de campanha
};
```

---

## 🔄 Como funciona o Upsert

O script **não duplica dados**. A cada execução:
- Verifica registros existentes pela chave `Data + ID do Item`
- Se já existir → **atualiza** a linha
- Se for novo → **insere** ao final

Isso garante que os últimos 15 dias estejam sempre com dados atualizados.

---

## ❓ Troubleshooting

**Dashboard mostra erro de carregamento:**
- Verifique se a planilha está pública (Leitor)
- Confirme o ID no `index.html`
- Confirme o nome da aba (`dados_brutos`)

**Script não encontra campanhas:**
- Verifique se o nome das campanhas contém "ecomm" ou "[ecomm]"
- Teste sem o filtro de campanha primeiro para ver se há dados

**Categorias aparecem vazias:**
- Verifique se o feed do Merchant Center tem `product_type` preenchido
- No Google Ads, confirme em Produtos > Atributos do Feed
