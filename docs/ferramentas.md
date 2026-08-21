# Ferramentas

CRM expõe 30 ferramentas.

### 1. `crm_describe`
**Input**: `account` (opcional)

Descreve o schema do CRM: todos os objetos (contact, company, deal e quaisquer custom) com seus campos (key, tipo, obrigatório, opções, relações) e o kanban.

### 2. `crm_list_objects`
**Input**: `account` (opcional)

Lista os objetos do CRM (key, rótulo, módulo, kanban).

### 3. `crm_create_records`
**Input**: `object`, `data` (opcional), `metadata` (opcional), `records` (opcional), `account` (opcional)

Cria um registro (ou vários) de um objeto.

### 4. `crm_list_records`
**Input**: `object`, `filter` (opcional), `sort` (opcional), `limit` (opcional), `offset` (opcional), `account` (opcional)

Lista registros de um objeto, com filtro/ordenação/paginação.

### 5. `crm_get_records`
**Input**: `object`, `ids`, `account` (opcional)

Busca um ou mais registros de um objeto por id.

### 6. `crm_update_records`
**Input**: `object`, `id` (opcional), `data` (opcional), `metadata` (opcional), `updates` (opcional), `account` (opcional), `ids` (opcional)

Atualiza um registro (merge parcial) por id.

### 7. `crm_delete_records`
**Input**: `object`, `ids`, `account` (opcional)

Remove um ou mais registros de um objeto por id.

### 8. `crm_search`
**Input**: `q`, `object` (opcional), `limit` (opcional), `account` (opcional)

Busca textual nos registros (por nome/título/valores).

### 9. `crm_define_object`
**Input**: `key`, `label` (opcional), `module` (opcional), `display_field` (opcional), `fields` (opcional), `account` (opcional)

Cria um OBJETO custom no CRM (além de contact/company/deal) e ganha CRUD/API automático.

### 10. `crm_add_field`
**Input**: `object`, `key`, `label` (opcional), `type`, `required` (opcional), `unique` (opcional), `indexed` (opcional), `options` (opcional), `relation` (opcional), `account` (opcional)

Adiciona um CAMPO custom a um objeto (sem migração; registros antigos ficam sem a key).

### 11. `crm_get_object`
**Input**: `object`, `account` (opcional)

Detalha UM objeto: seu rótulo, kanban e todos os campos (key, tipo, obrigatório, opções, relação).

### 12. `crm_update_object`
**Input**: `object`, `label` (opcional), `display_field` (opcional), `module` (opcional), `kanban` (opcional), `account` (opcional)

Edita um objeto: rótulo, campo de exibição, módulo ou config de kanban (a key é imutável).

### 13. `crm_delete_object`
**Input**: `object`, `delete_records` (opcional), `account` (opcional)

Remove um objeto CUSTOM (objetos de sistema não podem ser removidos).

### 14. `crm_update_field`
**Input**: `object`, `key`, `type` (opcional), `label` (opcional), `required` (opcional), `visible` (opcional), `position` (opcional), `indexed` (opcional), `options` (opcional), `account` (opcional)

Edita um campo: rótulo, obrigatório, visível, posição, opções, indexação.

### 15. `crm_remove_field`
**Input**: `object`, `key`, `account` (opcional)

Remove um campo CUSTOM (soft-delete: some do schema, mas os valores já gravados ficam preservados).

### 16. `crm_link_record`
**Input**: `object`, `id`, `field`, `target_id`, `account` (opcional), `ids` (opcional), `target_ids` (opcional)

Liga um registro a outro por um campo de relação (ex.: vincular um contato a uma empresa).

### 17. `crm_unlink_record`
**Input**: `object`, `id`, `field`, `target_id` (opcional), `account` (opcional), `ids` (opcional), `target_ids` (opcional)

Desfaz uma relação. Para relação 'many' remove o target_id do conjunto; para 'single' limpa o campo (target_id opcional). Bulk support: accepts ids, target_ids for batched execution.

### 18. `crm_related_records`
**Input**: `object`, `id`, `field` (opcional), `account` (opcional), `ids` (opcional)

Resolve os registros relacionados de um registro: segue os campos de relação e devolve os registros-alvo (todos, ou só de um `field`).

### 19. `crm_move_stage`
**Input**: `object`, `id`, `stage`, `account` (opcional), `ids` (opcional)

Move um registro de um objeto kanban (ex.: deal) para outra etapa do funil.

### 20. `crm_upload_file`
**Input**: `filename`, `size`, `content_type` (opcional), `object` (opcional), `record_id` (opcional), `account` (opcional), `record_ids` (opcional)

Inicia o upload de um arquivo (até 200 MB) e devolve uma URL presigned (`upload_url`) pra fazer o PUT direto no storage, mais o `file_id`.

### 21. `crm_confirm_upload`
**Input**: `file_id`, `account` (opcional), `file_ids` (opcional)

Confirma o upload depois do PUT na upload_url (o storage valida o tamanho real).

### 22. `crm_upload_from_url`
**Input**: `url`, `filename` (opcional), `content_type` (opcional), `object` (opcional), `record_id` (opcional), `account` (opcional), `record_ids` (opcional)

Sobe um arquivo A PARTIR DE UMA URL (a plataforma baixa e armazena, até 200 MB) — o jeito do AGENTE subir via API sem segurar os bytes.

### 23. `crm_list_files`
**Input**: `object` (opcional), `record_id` (opcional), `account` (opcional), `record_ids` (opcional)

Lista os arquivos do CRM (id, nome, tamanho, status).

### 24. `crm_get_file`
**Input**: `file_id`, `account` (opcional), `file_ids` (opcional)

Metadados de um arquivo (nome, tamanho, content_type, status, anexo).

### 25. `crm_download_file`
**Input**: `file_id`, `account` (opcional), `file_ids` (opcional)

Gera uma URL presigned de download (GET) pra um arquivo.

### 26. `crm_delete_file`
**Input**: `ids` (opcional), `file_id` (opcional), `account` (opcional)

Remove um ou mais arquivos (apaga do storage e libera o espaço contabilizado).

### 27. `crm_publish_file`
**Input**: `file_id`, `account` (opcional), `file_ids` (opcional)

Torna um arquivo PÚBLICO e devolve uma URL estável (`https://api.mcp.ai/f/<token>`) pra embutir num site (ex.: thumbnail).

### 28. `crm_unpublish_file`
**Input**: `file_id`, `account` (opcional), `file_ids` (opcional)

Revoga o link público de um arquivo (o token para de funcionar na hora).

### 29. `crm_usage`
**Input**: `account` (opcional)

Uso de storage deste CRM: bytes do banco + bytes de arquivos, total em GB, nº de arquivos e egress acumulado.

### 30. `crm_list_accounts`
**Input**: nenhum input

Lista os CRMs conectados neste install.

## Prompts de exemplo

```
Liste meus negócios em aberto no funil
Crie um contato Ada Lovelace com email ada@math.org
Adicione um campo personalizado 'origem' aos contatos
```
