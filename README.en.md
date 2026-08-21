# CRM

### CRM for Claude, ChatGPT and AI agents

Your own CRM, fully customizable and agent-driven. Start with contacts, companies and a sales pipeline, then create your own objects and fields and get an API instantly. You own all the data, isolated in a database of your own. Pay only for what you store and use.

- 📊 **30 tools**
- ✏️ **Read and write**
- 💬 **Works with any MCP client**: Claude Desktop, Cursor, VS Code, Cline, Continue
- 🔑 **Magic-link login (no password)**

[Portuguese version](README.md) · [Full docs (PT-BR)](docs/)

---

## One-click install

### Claude (Web and Desktop)

[➕ Open in Claude and connect](https://claude.ai/new?modal=add-custom-connector#settings/customize-connectors)

Manual: [claude.ai/customize/connectors](https://claude.ai/customize/connectors?surface=cowork) → **+** → **Add custom connector** → name `CRM`, URL `https://api.mcp.ai/p_crm`.

### Cursor

[➕ Install in Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=crm&config=eyJ1cmwiOiJodHRwczovL2FwaS5tY3AuYWkvcF9jcm0ifQ==)

### VS Code (Copilot Chat)

[➕ Install in VS Code](vscode:mcp/install?name=crm&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.mcp.ai%2Fp_crm%22%7D)

### Any other MCP-over-HTTP client

```
https://api.mcp.ai/p_crm
```

---

## 30 tools

| Tool | Description |
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

---

## Pricing

See [docs/precos.md](docs/precos.md) (PT-BR).

---

## License

MIT — see [LICENSE](LICENSE). The MCP server at `api.mcp.ai/p_crm` is proprietary (hosted); this repo (manifests, docs, skills) is MIT.
