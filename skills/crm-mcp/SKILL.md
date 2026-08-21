---
name: crm-mcp
description: Skill da REST API do CRM na MCP.AI: 30 endpoints em /api/crm. Seu CRM próprio, totalmente customizável e dirigido por agentes de IA. Comece com contatos, empresas e funil de vendas, depois crie seus próprios objetos e campos e ganhe API na hora. Você é dono de todos os dados, isolados num banco só seu. Paga só pelo que armazena e usa. Autentique com workspace API key (sk_live) gerada em app.mcp.ai/settings/api-keys. Use quando o usuário pedir algo coberto pelos endpoints.
---

# CRM — REST API skill

Você tem acesso à **CRM** REST API na MCP.AI.

> Seu CRM próprio, totalmente customizável e dirigido por agentes de IA. Comece com contatos, empresas e funil de vendas, depois crie seus próprios objetos e campos e ganhe API na hora. Você é dono de todos os dados, isolados num banco só seu. Paga só pelo que armazena e usa.

## Base URL

```
https://api.mcp.ai/api/crm
```

Todo endpoint é um **POST** na Base URL + o path abaixo. Os parâmetros vão no corpo JSON.

## Autenticação

Inclua em toda request:

```
Authorization: Bearer sk_live_...
Content-Type: application/json
```

> Gere sua chave em **https://app.mcp.ai/settings/api-keys** (workspace API key `sk_live_…`, não expira, revogável). Uma única chave serve pra todos os seus MCPs.

## Formato de resposta

```json
{ "ok": true, "tool": "<tool_id>", "result": <payload> }
```

## Exemplo cURL

```bash
curl -X POST https://api.mcp.ai/api/crm/add/field \
  -H "Authorization: Bearer sk_live_..." \
  -H "Content-Type: application/json" \
  -d '{"object":"...","key":"...","type":"..."}'
```

## Reportar problemas

Se um endpoint retornar erro, vazio ou dado inesperado, reporte (não desista calado): **POST /api/crm/report** com `{ "message": "...", "context"?: "...", "conversation"?: [...] }`. Isso notifica o time da MCP.AI.

## Endpoints (30)

#### `crm_add_field`

Adiciona um CAMPO custom a um objeto (sem migração; registros antigos ficam sem a key). _(POST /api/crm/add/field)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto que recebe o campo. |
| `key` | string | Sim | API name do campo (a-z/0-9/_). |
| `label` | string | Não | Rótulo do campo. |
| `type` | string | Sim | Tipo do campo (ver lista na descrição). |
| `required` | boolean | Não | Obrigatório? |
| `unique` | boolean | Não | Valor único no objeto? |
| `indexed` | boolean | Não | Indexar para filtro/busca rápida? |
| `options` | string | Não | Para select: array JSON de {value,label}, ex.: '[{"value":"a","label":"A"}]'. |
| `relation` | string | Não | Para type=relation: objeto JSON {"object_key":"company","many":false}. |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_confirm_upload`

Confirma o upload depois do PUT na upload_url (o storage valida o tamanho real). _(POST /api/crm/confirm/upload)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `file_id` | string | Sim | ID do arquivo (de crm_upload_file). |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |
| `file_ids` | string[] | Não | Bulk mode: multiple values for file_id |

#### `crm_create_records`

Cria um registro (ou vários) de um objeto. _(POST /api/crm/create/records)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto (ex.: contact, company, deal, ou um objeto custom). |
| `data` | string | Não | Campos do registro por key. Envie um objeto JSON (ex.: '{"name":"Ada","email":"ada@x.com"}'). |
| `metadata` | string | Não | Mapa livre não-tipado (opcional). Envie um objeto JSON (ex.: '{"name":"Ada","email":"ada@x.com"}'). |
| `records` | string | Não | Para criar vários: array JSON de objetos {data, metadata?} (ex.: '[{"data":{"name":"A"}}]'). |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_define_object`

Cria um OBJETO custom no CRM (além de contact/company/deal) e ganha CRUD/API automático. _(POST /api/crm/define/object)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `key` | string | Sim | API name do objeto (ex.: 'ticket', 'projeto'). |
| `label` | string | Não | Rótulo singular, ou objeto JSON {"singular","plural"}. |
| `module` | string | Não | Módulo (default 'custom'). |
| `display_field` | string | Não | Key do campo que rotula um registro nas listas. |
| `fields` | string | Não | Array JSON de campos, ex.: '[{"key":"subject","label":"Assunto","type":"string","required":true}]'. |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_delete_file`

Remove um ou mais arquivos (apaga do storage e libera o espaço contabilizado). _(POST /api/crm/delete/file)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `ids` | string[] | Não | IDs dos arquivos a remover. |
| `file_id` | string | Não | ID único (alternativa a ids). |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_delete_object`

Remove um objeto CUSTOM (objetos de sistema não podem ser removidos). _(POST /api/crm/delete/object)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto custom a remover. |
| `delete_records` | boolean | Não | Apagar também os registros do objeto. |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_delete_records`

Remove um ou mais registros de um objeto por id. _(POST /api/crm/delete/records)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto. |
| `ids` | string[] | Sim | IDs dos registros a remover. |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_describe`

Descreve o schema do CRM: todos os objetos (contact, company, deal e quaisquer custom) com seus campos (key, tipo, obrigatório, opções, relações) e o kanban. _(POST /api/crm/describe)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_download_file`

Gera uma URL presigned de download (GET) pra um arquivo. _(POST /api/crm/download/file)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `file_id` | string | Sim | ID do arquivo. |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |
| `file_ids` | string[] | Não | Bulk mode: multiple values for file_id |

#### `crm_get_file`

Metadados de um arquivo (nome, tamanho, content_type, status, anexo). _(POST /api/crm/get/file)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `file_id` | string | Sim | ID do arquivo. |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |
| `file_ids` | string[] | Não | Bulk mode: multiple values for file_id |

#### `crm_get_object`

Detalha UM objeto: seu rótulo, kanban e todos os campos (key, tipo, obrigatório, opções, relação). _(POST /api/crm/get/object)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto. |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_get_records`

Busca um ou mais registros de um objeto por id. _(POST /api/crm/get/records)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto. |
| `ids` | string[] | Sim | IDs dos registros (rec_...). |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_link_record`

Liga um registro a outro por um campo de relação (ex.: vincular um contato a uma empresa). _(POST /api/crm/link/record)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto de origem. |
| `id` | string | Sim | ID do registro de origem (rec_...). |
| `field` | string | Sim | Key do campo de relação. |
| `target_id` | string | Sim | ID do registro alvo (rec_...). |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |
| `target_ids` | string[] | Não | Bulk mode: multiple values for target_id |

#### `crm_list_accounts`

Lista os CRMs conectados neste install. _(POST /api/crm/list/accounts)_

#### `crm_list_files`

Lista os arquivos do CRM (id, nome, tamanho, status). _(POST /api/crm/list/files)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Não | Filtra por objeto (opcional). |
| `record_id` | string | Não | Filtra por registro (opcional). |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |
| `record_ids` | string[] | Não | Bulk mode: multiple values for record_id |

#### `crm_list_objects`

Lista os objetos do CRM (key, rótulo, módulo, kanban). _(POST /api/crm/list/objects)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_list_records`

Lista registros de um objeto, com filtro/ordenação/paginação. _(POST /api/crm/list/records)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto. |
| `filter` | string | Não | Filtro por campos. Envie um objeto JSON (ex.: '{"name":"Ada","email":"ada@x.com"}'). |
| `sort` | string | Não | Ordenação, ex.: {"updated_at":-1} ou {"value":-1}. Envie um objeto JSON (ex.: '{"name":"Ada","email":"ada@x.com"}'). |
| `limit` | integer | Não | Máx. por página (default 50). |
| `offset` | integer | Não | Deslocamento para paginar. |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_move_stage`

Move um registro de um objeto kanban (ex.: deal) para outra etapa do funil. _(POST /api/crm/move/stage)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto kanban (ex.: deal). |
| `id` | string | Sim | ID do registro (rec_...). |
| `stage` | string | Sim | Nova etapa (value da opção do campo de stage). |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `crm_publish_file`

Torna um arquivo PÚBLICO e devolve uma URL estável (`https://api.mcp.ai/f/<token>`) pra embutir num site (ex.: thumbnail). _(POST /api/crm/publish/file)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `file_id` | string | Sim | ID do arquivo (precisa estar 'ready'). |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |
| `file_ids` | string[] | Não | Bulk mode: multiple values for file_id |

#### `crm_related_records`

Resolve os registros relacionados de um registro: segue os campos de relação e devolve os registros-alvo (todos, ou só de um `field`). _(POST /api/crm/related/records)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto. |
| `id` | string | Sim | ID do registro (rec_...). |
| `field` | string | Não | Restringe a um campo de relação (opcional). |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `crm_remove_field`

Remove um campo CUSTOM (soft-delete: some do schema, mas os valores já gravados ficam preservados). _(POST /api/crm/remove/field)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto. |
| `key` | string | Sim | Key do campo custom a remover. |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_search`

Busca textual nos registros (por nome/título/valores). _(POST /api/crm/search)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `q` | string | Sim | Termo de busca. |
| `object` | string | Não | Restringe a um objeto (opcional). |
| `limit` | integer | Não | Máx. de resultados (default 50). |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_unlink_record`

Desfaz uma relação. Para relação 'many' remove o target_id do conjunto; para 'single' limpa o campo (target_id opcional). _(POST /api/crm/unlink/record)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto de origem. |
| `id` | string | Sim | ID do registro de origem (rec_...). |
| `field` | string | Sim | Key do campo de relação. |
| `target_id` | string | Não | ID do alvo a remover (necessário em relação many). |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |
| `target_ids` | string[] | Não | Bulk mode: multiple values for target_id |

#### `crm_unpublish_file`

Revoga o link público de um arquivo (o token para de funcionar na hora). _(POST /api/crm/unpublish/file)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `file_id` | string | Sim | ID do arquivo. |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |
| `file_ids` | string[] | Não | Bulk mode: multiple values for file_id |

#### `crm_update_field`

Edita um campo: rótulo, obrigatório, visível, posição, opções, indexação. _(POST /api/crm/update/field)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto. |
| `key` | string | Sim | Key do campo a editar. |
| `type` | string | Não | Novo tipo (só alargamento seguro). |
| `label` | string | Não | Novo rótulo. |
| `required` | boolean | Não | Obrigatório? |
| `visible` | boolean | Não | Visível? |
| `position` | integer | Não | Posição/ordem. |
| `indexed` | boolean | Não | Indexar? |
| `options` | string | Não | Para select: array JSON de {value,label}. |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_update_object`

Edita um objeto: rótulo, campo de exibição, módulo ou config de kanban (a key é imutável). _(POST /api/crm/update/object)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto a editar. |
| `label` | string | Não | Novo rótulo singular, ou objeto JSON {"singular","plural"}. |
| `display_field` | string | Não | Key do campo que rotula um registro. |
| `module` | string | Não | Módulo. |
| `kanban` | string | Não | Config de kanban como JSON, ex.: {"stage_field":"stage"}. |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

#### `crm_update_records`

Atualiza um registro (merge parcial) por id. _(POST /api/crm/update/records)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `object` | string | Sim | Key do objeto. |
| `id` | string | Não | ID do registro a atualizar (rec_...). |
| `data` | string | Não | Campos a atualizar. Envie um objeto JSON (ex.: '{"name":"Ada","email":"ada@x.com"}'). |
| `metadata` | string | Não | Metadata livre a mesclar (null numa chave apaga). Envie um objeto JSON (ex.: '{"name":"Ada","email":"ada@x.com"}'). |
| `updates` | string | Não | Para vários: array JSON de {id, data, metadata?}. |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |
| `ids` | string[] | Não | Bulk mode: multiple values for id |

#### `crm_upload_file`

Inicia o upload de um arquivo (até 200 MB) e devolve uma URL presigned (`upload_url`) pra fazer o PUT direto no storage, mais o `file_id`. _(POST /api/crm/upload/file)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `filename` | string | Sim | Nome do arquivo. |
| `size` | integer | Sim | Tamanho em bytes (≤ 200 MB). |
| `content_type` | string | Não | MIME type (ex.: application/pdf). |
| `object` | string | Não | Key do objeto ao qual anexar (opcional). |
| `record_id` | string | Não | ID do registro ao qual anexar (opcional). |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |
| `record_ids` | string[] | Não | Bulk mode: multiple values for record_id |

#### `crm_upload_from_url`

Sobe um arquivo A PARTIR DE UMA URL (a plataforma baixa e armazena, até 200 MB) — o jeito do AGENTE subir via API sem segurar os bytes. _(POST /api/crm/upload/from/url)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `url` | string | Sim | URL http(s) do arquivo a baixar e armazenar. |
| `filename` | string | Não | Nome do arquivo (inferido da URL se omitido). |
| `content_type` | string | Não | MIME type (inferido do response se omitido). |
| `object` | string | Não | Key do objeto ao qual anexar (opcional). |
| `record_id` | string | Não | ID do registro ao qual anexar (opcional). |
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |
| `record_ids` | string[] | Não | Bulk mode: multiple values for record_id |

#### `crm_usage`

Uso de storage deste CRM: bytes do banco + bytes de arquivos, total em GB, nº de arquivos e egress acumulado. _(POST /api/crm/usage)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `account` | string | Não | Seletor opcional do CRM quando há mais de um conectado neste install. Passe o id ou o rótulo (match parcial). Omita se só há um. Use crm_list_accounts para descobrir. |

---

Este MCP também funciona via **conexão MCP** (Claude / Cursor) em `https://api.mcp.ai/p_crm` — veja o [README](../../README.md). A skill acima é pra consumir a **REST API** direto (agente próprio / código).
