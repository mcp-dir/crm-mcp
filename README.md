# CRM

### CRM para Claude, ChatGPT e agentes de IA

Seu CRM próprio, totalmente customizável e dirigido por agentes de IA. Comece com contatos, empresas e funil de vendas, depois crie seus próprios objetos e campos e ganhe API na hora. Você é dono de todos os dados, isolados num banco só seu. Paga só pelo que armazena e usa.

- 📊 **30 ferramentas**
- ✏️ **Leitura e escrita**
- 💬 **Funciona com qualquer cliente MCP**: Claude Desktop, Cursor, VS Code, Cline, Continue
- 🔑 **Login via magic-link (sem senha)**

[English version](README.en.md) · [Documentação completa](docs/) · [Skill pra agentes](skills/)

---

## Instalar em 1 clique

### Claude (Web e Desktop)

A Anthropic unificou a instalação de MCPs em `claude.ai/customize/connectors`. **O mesmo link serve pra Claude Web e Claude Desktop** (basta estar logado):

[➕ Abrir no Claude e conectar](https://claude.ai/new?modal=add-custom-connector#settings/customize-connectors)

**Manual** (se o deeplink não abrir): [claude.ai/customize/connectors](https://claude.ai/customize/connectors?surface=cowork) → **+** → **Adicionar conector personalizado** → cole **Nome** `CRM` e **URL** `https://api.mcp.ai/p_crm`.

### Cursor

[➕ Instalar CRM no Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=crm&config=eyJ1cmwiOiJodHRwczovL2FwaS5tY3AuYWkvcF9jcm0ifQ==)

### VS Code (Copilot Chat)

[➕ Instalar CRM no VS Code](vscode:mcp/install?name=crm&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.mcp.ai%2Fp_crm%22%7D)

### ChatGPT, Manus, OpenClaw e mais 40+ clientes

Funciona em qualquer cliente MCP que suporte **MCP over HTTP**. A URL do servidor é sempre:

```
https://api.mcp.ai/p_crm
```

Detalhes por cliente: [INSTALL.md](INSTALL.md).

---

## Exemplos de uso

```
Liste meus negócios em aberto no funil
Crie um contato Ada Lovelace com email ada@math.org
Adicione um campo personalizado 'origem' aos contatos
```

---

## 30 ferramentas disponíveis

| Tool | Descrição |
|---|---|
| `crm_describe` | Descreve o schema do CRM: todos os objetos (contact, company, deal e quaisquer custom) com seus campos (key, tipo, obrigatório, opções, relações) e o kanban. |
| `crm_list_objects` | Lista os objetos do CRM (key, rótulo, módulo, kanban). |
| `crm_create_records` | Cria um registro (ou vários) de um objeto. |
| `crm_list_records` | Lista registros de um objeto, com filtro/ordenação/paginação. |
| `crm_get_records` | Busca um ou mais registros de um objeto por id. |
| `crm_update_records` | Atualiza um registro (merge parcial) por id. |
| `crm_delete_records` | Remove um ou mais registros de um objeto por id. |
| `crm_search` | Busca textual nos registros (por nome/título/valores). |
| `crm_define_object` | Cria um OBJETO custom no CRM (além de contact/company/deal) e ganha CRUD/API automático. |
| `crm_add_field` | Adiciona um CAMPO custom a um objeto (sem migração; registros antigos ficam sem a key). |
| `crm_get_object` | Detalha UM objeto: seu rótulo, kanban e todos os campos (key, tipo, obrigatório, opções, relação). |
| `crm_update_object` | Edita um objeto: rótulo, campo de exibição, módulo ou config de kanban (a key é imutável). |
| `crm_delete_object` | Remove um objeto CUSTOM (objetos de sistema não podem ser removidos). |
| `crm_update_field` | Edita um campo: rótulo, obrigatório, visível, posição, opções, indexação. |
| `crm_remove_field` | Remove um campo CUSTOM (soft-delete: some do schema, mas os valores já gravados ficam preservados). |
| `crm_link_record` | Liga um registro a outro por um campo de relação (ex.: vincular um contato a uma empresa). |
| `crm_unlink_record` | Desfaz uma relação. Para relação 'many' remove o target_id do conjunto; para 'single' limpa o campo (target_id opcional). Bulk support: accepts ids, target_ids for batched execution. |
| `crm_related_records` | Resolve os registros relacionados de um registro: segue os campos de relação e devolve os registros-alvo (todos, ou só de um `field`). |
| `crm_move_stage` | Move um registro de um objeto kanban (ex.: deal) para outra etapa do funil. |
| `crm_upload_file` | Inicia o upload de um arquivo (até 200 MB) e devolve uma URL presigned (`upload_url`) pra fazer o PUT direto no storage, mais o `file_id`. |
| `crm_confirm_upload` | Confirma o upload depois do PUT na upload_url (o storage valida o tamanho real). |
| `crm_upload_from_url` | Sobe um arquivo A PARTIR DE UMA URL (a plataforma baixa e armazena, até 200 MB) — o jeito do AGENTE subir via API sem segurar os bytes. |
| `crm_list_files` | Lista os arquivos do CRM (id, nome, tamanho, status). |
| `crm_get_file` | Metadados de um arquivo (nome, tamanho, content_type, status, anexo). |
| `crm_download_file` | Gera uma URL presigned de download (GET) pra um arquivo. |
| `crm_delete_file` | Remove um ou mais arquivos (apaga do storage e libera o espaço contabilizado). |
| `crm_publish_file` | Torna um arquivo PÚBLICO e devolve uma URL estável (`https://api.mcp.ai/f/<token>`) pra embutir num site (ex.: thumbnail). |
| `crm_unpublish_file` | Revoga o link público de um arquivo (o token para de funcionar na hora). |
| `crm_usage` | Uso de storage deste CRM: bytes do banco + bytes de arquivos, total em GB, nº de arquivos e egress acumulado. |
| `crm_list_accounts` | Lista os CRMs conectados neste install. |

Detalhe de cada tool: [docs/ferramentas.md](docs/ferramentas.md)

---

## Preços

Veja [docs/precos.md](docs/precos.md).

---

## Privacidade & LGPD

- **Sub-processadores**: o LLM host que você escolher (Claude, ChatGPT, Cursor, agente próprio). Lista completa em [docs/privacidade-lgpd.md](docs/privacidade-lgpd.md).
- Os dados retornados pelas tools são enviados ao **LLM host que você escolher**, sub-processador fora do nosso controle. Recomendamos planos com opt-out de treinamento.

---

## Perguntas frequentes

**O servidor é open source?**
O servidor é proprietário (hosted). Este repositório é o wrapper público com manifestos, docs e skills — tudo MIT.

**Posso usar com agente próprio (não Claude/Cursor)?**
Sim — qualquer cliente que suporte MCP over HTTP. URL: `https://api.mcp.ai/p_crm`.


---

## Suporte

- 📧 [crm@mcp.ai](mailto:crm@mcp.ai)
- 🐛 [GitHub Issues](https://github.com/mcp-dir/crm-mcp/issues)
- 📄 [docs/](docs/)

---

## Licença

MIT — veja [LICENSE](LICENSE). O servidor MCP em `api.mcp.ai/p_crm` é proprietário (hosted); este repositório (manifestos, docs, skills) é MIT.
