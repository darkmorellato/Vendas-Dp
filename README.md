# Dashboard de Vendas — DP Analytics

Este dashboard exibe análises de vendas por período (mês). A estrutura de dados otimizada permite editar cada mês separadamente sem precisar gerar um arquivo consolidado.

## 📁 Estrutura de pastas

```
/
├── index.html          # Dashboard principal
└── dados/
    ├── ordem.json      # Ordem de exibição dos meses
    ├── jan2026.json    # Dados de Janeiro 2026
    ├── fev2026.json    # Dados de Fevereiro 2026
    ├── mar2026.json    # ...
    ├── abr2026.json
    ├── mai2026.json
    ├── dez2025.json
    ├── nov2025.json
    └── out2025.json
```

## ✏️ Como atualizar ou adicionar dados

### 1. Editar um mês existente
Abra o arquivo correspondente em `dados/` (ex: `dados/mai2026.json`) e modifique os campos:

```json
{
  "label": "Maio 2026",
  "total": 232,
  "data": [
    { "name": "PRODUTO X", "quantity": 10, "percentage": "0,43%" },
    ...
  ]
}
```

- `label`: texto que aparecerá no seletor de período.
- `total`: quantidade total de itens (pode ser recalculado somando `quantity`).
- `data`: array com os produtos e suas quantidades/porcentagens.

### 2. Adicionar um novo mês
1. Crie um novo arquivo `dados/JUN2026.json` (use o formato `mmmAAAA`).
2. Cole o conteúdo seguindo a mesma estrutura.
3. **Importante:** edite `dados/ordem.json`:
   - Adicione a nova chave no **início** da lista `_ordem_exibicao` para aparecer como primeiro mês no seletor.
   - Exemplo:
     ```json
     {
       "_ordem_exibicao": [
         "jun2026",
         "mai2026",
         "abr2026",
         ...
       ]
     }
     ```

## ▶️ Como executar

Basta abrir `index.html` em um navegador. Não é necessário servidor local; porém alguns navegadores bloqueiam `fetch` em arquivos locais. Se encontrar erro de carregamento, use uma extensão como **Live Server** (VSCode) ou execute um servidor simples:

```bash
python -m http.server 8000
```

Depois acesse `http://localhost:8000`.

## 🔄 Reconstruir `data.json` (opcional)

Se preferir usar um único arquivo consolidado, crie um pequeno script `build.py` que reúna tudo em `data.json`:

```python
import json, os
ordem = json.load(open('dados/ordem.json'))['_ordem_exibicao']
out = { "_ordem_exibicao": ordem }
for mes in ordem:
    with open(f'dados/{mes}.json') as f:
        out[mes] = json.load(f)
json.dump(out, open('data.json', 'w'), ensure_ascii=False, indent=4)
```

Atualmente o dashboard lê direto da pasta `dados/`, então o `data.json` não é necessário.

## 📌 Notas

- Não altere os nomes das chaves (ex: `mai2026`), pois o código usa esses identifiers.
- Mantenha a consistência dos campos `label`, `total` e `data`.
- `percentage` é só informativa; não é usada nos cálculos.
