# API TECHCOOP — Documentação Completa (API V1)

## 1) Autenticação

### Headers obrigatórios
- `Authorization: Bearer <token>`
- `Account-Key: <account_key>`
- `App: <nome_app>` (lido no controller, pode ser validado por regra futura)

### Regras do `authenticate_account`
- Exige SSL (exceto `development`).
- Se faltar `Authorization`/`Account-Key` → `401 unauthorized`.
- Se token/chave inválidos → `401 unauthorized`.
- Se não for SSL em produção → `426 upgrade_required`.

### Exceção (gambiarra específica em `/api/v1/pedidos`)
Se a requisição for para `/api/v1/pedidos` sem `Authorization`, o código tenta extrair `account_id` do body e usar credenciais da conta.

---

## 2) Parâmetros globais (GET/listagem)

Aplicáveis aos endpoints `#show` (GET de coleção):

- `page` (int, default `1`)
- `per_page` (int, default `10`)
- `company_id` (filtra por `codigo`)
- `created_between[]` (array com 2 datas)
- `created_after` / `created_before`
- `updated_between[]` (array com 2 datas)
- `updated_after` / `updated_before`

### Resposta padrão (`json_api`)
```json
{
  "header": {
    "total_records": 123,
    "total_pages": 13,
    "current_page": 1
  },
  "items": [ ... ]
}
```

---

## 3) Parâmetros de escrita (POST/PATCH/PUT)

### POST / PATCH (`create_api`)
- Payload obrigatório:
```json
{
  "items": [
    { "codigo": "...", "...": "..." }
  ]
}
```
- Comportamento:
  - Upsert por `(codigo, account_id)`.
  - Injeta `account_id` da conta autenticada.
  - Sanitiza atributos:
    - `clone` → `clone_flag`
    - remove atributos desconhecidos do model
    - converte número string com vírgula (`"12,34"`) para ponto (`"12.34"`)

- Resposta:
  - `201 created` com itens `{id, codigo}`
  - `422 unprocessable_entity` em erro

### PUT (`delete_api`)
- Payload obrigatório:
```json
{
  "items": [
    { "codigo": "..." }
  ]
}
```
- Comportamento:
  - Deleta por `(codigo, account_id)`
- Resposta:
  - `201 created` com itens deletados/encontrados `{id, codigo}`
  - `id: 0` quando não encontrado
  - `422` em erro

---

## 4) Endpoints especiais (fora do padrão CRUD em lote)

1. `GET|POST /api/v1/close_pedidos` → `api/v1/gestao_orders#close_order`
2. `GET /api/v1/licencas` → `api/v1/accounts#show`
3. `GET /api/v1/contas` → `api/v1/accounts#all_account`
4. `GET /api/v1/gestao_stats` → `api/v1/gestao_stats#count_records`
5. `POST /api/v1/isepe` → `api/v1/isepe_aulas#create_multiple`
6. `PUT /api/v1/isepe` → `api/v1/isepe_aulas#create_multiple`
7. `GET /api/v1/isepe` → `api/v1/isepe_aulas#show`
8. `DELETE /api/v1/isepe/:id` → `api/v1/isepe_aulas#destroy`
9. `PUT /api/v1/dfes` → `api/v1/dves#delete_multiple`
10. `POST /api/v1/dfes` → `api/v1/dves#create_multiple`
11. `GET /api/v1/dfes` → `api/v1/dves#show`

---

## 5) Endpoints V1 completos por recurso

> Padrão majoritário por recurso:
> - `GET /api/v1/<recurso>` → `#show`
> - `POST /api/v1/<recurso>` → `#create_multiple`
> - `PATCH /api/v1/<recurso>` → `#create_multiple`
> - `PUT /api/v1/<recurso>` → `#delete_multiple`

### 5.1 Base comercial/estoque/cadastro

- `/api/v1/local_estoques` → `api/v1/gestao_local_estoques#show|create_multiple|delete_multiple`
- `/api/v1/pedidos` → `api/v1/gestao_orders#show|create_multiple|delete_multiple`
- `/api/v1/produtos` → `api/v1/gestao_produtos#show|create_multiple|delete_multiple`
- `/api/v1/produto_estoques` → `api/v1/gestao_produto_estoques#show|create_multiple|delete_multiple`
- `/api/v1/produto_codigos` → `api/v1/gestao_produto_codigos#show|create_multiple|delete_multiple`
- `/api/v1/produto_pessoas` → `api/v1/gestao_produto_pessoas#show|create_multiple|delete_multiple`
- `/api/v1/produto_caracteristicas` → `api/v1/gestao_produto_caracteristicas#show|create_multiple|delete_multiple`
- `/api/v1/produto_controles` → `api/v1/gestao_produto_controles#show|create_multiple|delete_multiple`
- `/api/v1/produto_formulas` → `api/v1/gestao_produto_formulas#show|create_multiple|delete_multiple`
- `/api/v1/produto_pacotes` → `api/v1/gestao_produto_pacotes#show|create_multiple|delete_multiple`
- `/api/v1/produto_reservas` → `api/v1/gestao_produto_reservas#show|create_multiple|delete_multiple`
- `/api/v1/produto_grades` → `api/v1/gestao_produto_grades#show|create_multiple|delete_multiple`
- `/api/v1/produto_similares` → `api/v1/gestao_produto_similars#show|create_multiple|delete_multiple`
- `/api/v1/unidades` → `api/v1/gestao_unidades#show|create_multiple|delete_multiple`
- `/api/v1/ncms` → `api/v1/gestao_ncms#show|create_multiple|delete_multiple`
- `/api/v1/tributacoes` → `api/v1/gestao_tributacaos#show|create_multiple|delete_multiple`
- `/api/v1/tributacao_itens` → `api/v1/gestao_tributacao_items#show|create_multiple|delete_multiple`
- `/api/v1/marcas` → `api/v1/gestao_marcas#show|create_multiple|delete_multiple`
- `/api/v1/grupos` → `api/v1/gestao_grupos#show|create_multiple|delete_multiple`
- `/api/v1/sub_grupos` → `api/v1/gestao_sub_grupos#show|create_multiple|delete_multiple`
- `/api/v1/usuarios` → `api/v1/gestao_usuarios#show|create_multiple|delete_multiple`
- `/api/v1/pessoas` → `api/v1/gestao_pessoas#show|create_multiple|delete_multiple`
- `/api/v1/pessoa_plano_vendas` → `api/v1/gestao_pessoa_plano_vendas#show|create_multiple|delete_multiple`
- `/api/v1/cidades` → `api/v1/gestao_cidades#show|create_multiple|delete_multiple`
- `/api/v1/configs` → `api/v1/gestao_configs#show|create_multiple|delete_multiple`
- `/api/v1/empresas` → `api/v1/gestao_empresas#show|create_multiple|delete_multiple`

### 5.2 Documentos fiscais / operações fiscais

- `/api/v1/documentos` → `api/v1/gestao_nfs#show|create_multiple|delete_multiple`
- `/api/v1/documento_origens` → `api/v1/gestao_nf_origens#show|create_multiple|delete_multiple`
- `/api/v1/documento_produtos` → `api/v1/gestao_nf_produtos#show|create_multiple|delete_multiple`
- `/api/v1/documento_produto_controles` → `api/v1/gestao_nf_produto_controles#show|create_multiple|delete_multiple`
- `/api/v1/documento_servicos` → `api/v1/gestao_nf_servicos#show|create_multiple|delete_multiple`
- `/api/v1/documento_operacoes` → `api/v1/gestao_nf_operacaos#show|create_multiple|delete_multiple`
- `/api/v1/documento_operacao_davs` → `api/v1/gestao_nf_operacao_davs#show|create_multiple|delete_multiple`
- `/api/v1/documento_faturas` → `api/v1/gestao_nf_faturas#show|create_multiple|delete_multiple`
- `/api/v1/documento_fatura_davs` → `api/v1/gestao_nf_fatura_davs#show|create_multiple|delete_multiple`
- `/api/v1/cfops` → `api/v1/gestao_cfops#show|create_multiple|delete_multiple`
- `/api/v1/modelos` → `api/v1/gestao_modelos#show|create_multiple|delete_multiple`
- `/api/v1/operacoes` → `api/v1/gestao_operacaos#show|create_multiple|delete_multiple`
- `/api/v1/fidelidades` → `api/v1/gestao_fidelidades#show|create_multiple|delete_multiple`
- `/api/v1/lista_preco_itens` → `api/v1/gestao_lista_preco_items#show|create_multiple|delete_multiple`
- `/api/v1/lista_precos` → `api/v1/gestao_lista_precos#show|create_multiple|delete_multiple`
- `/api/v1/plano_vendas` → `api/v1/gestao_plano_vendas#show|create_multiple|delete_multiple`
- `/api/v1/operacao_plano_vendas` → `api/v1/gestao_operacao_plano_vendas#show|create_multiple|delete_multiple`
- `/api/v1/operacao_plano_gs` → `api/v1/gestao_operacao_plano_gs#show|create_multiple|delete_multiple`
- `/api/v1/operacao_multis` → `api/v1/gestao_operacao_multis#show|create_multiple|delete_multiple`
- `/api/v1/operacao_bandeiras` → `api/v1/gestao_operacao_bandeiras#show|create_multiple|delete_multiple`
- `/api/v1/cfop_operacoes` → `api/v1/gestao_cfop_operacaos#show|create_multiple|delete_multiple`

### 5.3 Financeiro / movimento

- `/api/v1/duplicatas` → `api/v1/gestao_duplicata#show|create_multiple|delete_multiple`
- `/api/v1/duplicata_baixas` → `api/v1/gestao_duplicata_baixas#show|create_multiple|delete_multiple`
- `/api/v1/recibos` → `api/v1/gestao_recibos#show|create_multiple|delete_multiple`
- `/api/v1/movimentos` → `api/v1/gestao_movimentos#show|create_multiple|delete_multiple`
- `/api/v1/movimento_gerenciais` → `api/v1/gestao_movimento_gerencials#show|create_multiple|delete_multiple`
- `/api/v1/manifestos` → `api/v1/gestao_manifestos#show|create_multiple|delete_multiple`
- `/api/v1/ajustes` → `api/v1/gestao_ajustes#show|create_multiple|delete_multiple`
- `/api/v1/ajuste_itens` → `api/v1/gestao_ajuste_items#show|create_multiple|delete_multiple`
- `/api/v1/acumuladores` → `api/v1/gestao_acumuladors#show|create_multiple|delete_multiple`
- `/api/v1/precos` → `api/v1/gestao_precos#show|create_multiple|delete_multiple`
- `/api/v1/preco_itens` → `api/v1/gestao_preco_items#show|create_multiple|delete_multiple`

### 5.4 Produção / impressão

- `/api/v1/producoes` → `api/v1/gestao_producaos#show|create_multiple|delete_multiple`
- `/api/v1/producao_itens` → `api/v1/gestao_producao_items#show|create_multiple|delete_multiple`
- `/api/v1/producao_finais` → `api/v1/gestao_producao_finals#show|create_multiple|delete_multiple`
- `/api/v1/grupo_impressoes` → `api/v1/gestao_grupo_impressaos#show|create_multiple|delete_multiple`

### 5.5 Módulo animal/veterinário

- `/api/v1/animais` → `api/v1/gestao_animals#show|create_multiple|delete_multiple`
- `/api/v1/animal_exames` → `api/v1/gestao_animal_exames#show|create_multiple|delete_multiple`
- `/api/v1/animal_receitas` → `api/v1/gestao_animal_receita#show|create_multiple|delete_multiple`
- `/api/v1/animal_receita_itens` → `api/v1/gestao_animal_receita_items#show|create_multiple|delete_multiple`
- `/api/v1/animal_vacinas` → `api/v1/gestao_animal_vacinas#show|create_multiple|delete_multiple`
- `/api/v1/medicamento_usos` → `api/v1/gestao_medicamento_usos#show|create_multiple|delete_multiple`
- `/api/v1/vacinas` → `api/v1/gestao_vacinas#show|create_multiple|delete_multiple`
- `/api/v1/especies` → `api/v1/gestao_especies#show|create_multiple|delete_multiple`
- `/api/v1/racas` → `api/v1/gestao_racas#show|create_multiple|delete_multiple`
- `/api/v1/reservas` → `api/v1/gestao_reservas#show|create_multiple|delete_multiple`
- `/api/v1/reserva_itens` → `api/v1/gestao_reserva_items#show|create_multiple|delete_multiple`
- `/api/v1/romaneios` → `api/v1/gestao_romaneios#show|create_multiple|delete_multiple`
- `/api/v1/romaneio_itens` → `api/v1/gestao_romaneio_items#show|create_multiple|delete_multiple`
- `/api/v1/paf_davs` → `api/v1/gestao_paf_davs#show|create_multiple|delete_multiple`
- `/api/v1/pessoa_horarios` → `api/v1/gestao_pessoa_horarios#show|create_multiple|delete_multiple`

---

## 6) Matriz de métodos por URL (completa e explícita)

Para **cada URL abaixo**, valem estes métodos:
- `GET` → show
- `POST` → create_multiple
- `PATCH` → create_multiple
- `PUT` → delete_multiple

URLs:
`/api/v1/local_estoques`, `/api/v1/pedidos`, `/api/v1/produtos`, `/api/v1/produto_estoques`,
`/api/v1/produto_codigos`, `/api/v1/produto_pessoas`, `/api/v1/produto_caracteristicas`,
`/api/v1/produto_controles`, `/api/v1/produto_formulas`, `/api/v1/produto_pacotes`,
`/api/v1/produto_reservas`, `/api/v1/produto_grades`, `/api/v1/produto_similares`,
`/api/v1/unidades`, `/api/v1/ncms`, `/api/v1/tributacoes`, `/api/v1/tributacao_itens`,
`/api/v1/marcas`, `/api/v1/grupos`, `/api/v1/sub_grupos`, `/api/v1/usuarios`, `/api/v1/pessoas`,
`/api/v1/pessoa_plano_vendas`, `/api/v1/cidades`, `/api/v1/configs`, `/api/v1/empresas`,
`/api/v1/documentos`, `/api/v1/documento_origens`, `/api/v1/documento_produtos`,
`/api/v1/documento_produto_controles`, `/api/v1/documento_servicos`, `/api/v1/documento_operacoes`,
`/api/v1/documento_operacao_davs`, `/api/v1/documento_faturas`, `/api/v1/documento_fatura_davs`,
`/api/v1/cfops`, `/api/v1/modelos`, `/api/v1/operacoes`, `/api/v1/fidelidades`,
`/api/v1/lista_preco_itens`, `/api/v1/lista_precos`, `/api/v1/plano_vendas`,
`/api/v1/operacao_plano_vendas`, `/api/v1/operacao_plano_gs`, `/api/v1/operacao_multis`,
`/api/v1/operacao_bandeiras`, `/api/v1/cfop_operacoes`, `/api/v1/duplicatas`,
`/api/v1/duplicata_baixas`, `/api/v1/recibos`, `/api/v1/movimentos`, `/api/v1/movimento_gerenciais`,
`/api/v1/manifestos`, `/api/v1/ajustes`, `/api/v1/ajuste_itens`, `/api/v1/acumuladores`,
`/api/v1/precos`, `/api/v1/preco_itens`, `/api/v1/producoes`, `/api/v1/producao_itens`,
`/api/v1/producao_finais`, `/api/v1/grupo_impressoes`, `/api/v1/animais`, `/api/v1/animal_exames`,
`/api/v1/animal_receitas`, `/api/v1/animal_receita_itens`, `/api/v1/animal_vacinas`,
`/api/v1/medicamento_usos`, `/api/v1/vacinas`, `/api/v1/especies`, `/api/v1/racas`,
`/api/v1/reservas`, `/api/v1/reserva_itens`, `/api/v1/romaneios`, `/api/v1/romaneio_itens`,
`/api/v1/paf_davs`, `/api/v1/pessoa_horarios`.

**Exceções:** `close_pedidos`, `isepe`, `licencas`, `contas`, `gestao_stats`, `dfes`.

---

## 7) Exemplos prontos

### 7.1 GET com paginação e filtro temporal
```bash
curl -X GET "https://SEU_HOST/api/v1/produtos?page=1&per_page=50&updated_after=2026-01-01"   -H "Authorization: Bearer SEU_TOKEN"   -H "Account-Key: SUA_ACCOUNT_KEY"   -H "App: desktop"
```

### 7.2 POST/PATCH (upsert em lote)
```bash
curl -X POST "https://SEU_HOST/api/v1/produtos"   -H "Content-Type: application/json"   -H "Authorization: Bearer SEU_TOKEN"   -H "Account-Key: SUA_ACCOUNT_KEY"   -H "App: desktop"   -d '{
    "items": [
      { "codigo": "P001", "descricao": "Produto 1", "preco": "12,34", "clone": true },
      { "codigo": "P002", "descricao": "Produto 2", "preco": "99,90" }
    ]
  }'
```

### 7.3 PUT (deleção por código da conta)
```bash
curl -X PUT "https://SEU_HOST/api/v1/produtos"   -H "Content-Type: application/json"   -H "Authorization: Bearer SEU_TOKEN"   -H "Account-Key: SUA_ACCOUNT_KEY"   -H "App: desktop"   -d '{
    "items": [
      { "codigo": "P001" },
      { "codigo": "P999" }
    ]
  }'
```

---
