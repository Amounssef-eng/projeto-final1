# 📦 Feature: Análise de Itens Similares no Inventário

## Descrição

Nova funcionalidade que analisa automaticamente itens que são parecidos e iguais dentro do mesmo **slot** do inventário, facilitando a detecção de duplicatas e organizando melhor o estoque.

## Como Funciona

### 1. **Conceito de Slot**
- Cada produto agora possui um **slot**, que representa uma localização ou categoria de armazenamento
- Exemplos: `PRATELEIRA_A1`, `GAVETA_03`, `ARMARIO_ENTRADA`, etc.

### 2. **Detecção de Similares**
A sistema usa o **algoritmo de Levenshtein** para calcular a similaridade entre nomes de produtos:
- **100% de similaridade**: Produtos com nomes idênticos (IGUAIS)
- **≥70% similaridade**: Produtos com nomes parecidos (SIMILARES)
- **<70% similaridade**: Produtos diferentes (não exibidos como similar)

### 3. **Avisos no Frontend**
Cada card de produto mostra:

#### Badge do Slot
```
📍 Slot: PRATELEIRA_A1
```

#### Aviso de Itens IDÊNTICOS (Red)
```
⚠️ IDÊNTICOS: 2 item(ns) igual(is) no mesmo slot!
• Notebook Dell (5 un, €450.00)
• Notebook HP (3 un, €520.00)
```

#### Aviso de Itens SIMILARES (Green)
```
📌 SIMILARES: 1 item(ns) parecido(s) no slot
• Teclado Mecânico (85% similar)
```

## Uso

### Frontend (Offline)

1. **Adicionar um produto com slot:**
   - Preenca o campo "📍 Slot" no formulário
   - Exemplo: `PRATELEIRA_A`, `CAIXA_001`, etc.

2. **Visualizar similares:**
   - Os avisos aparecem automaticamente no card do produto
   - Se houver itens iguais ou similares no mesmo slot, uma notificação será exibida

### Backend (API REST)

#### Endpoints Disponíveis

**1. GET /produtos**
- Retorna todos os produtos com o campo `slot`

**2. GET /produtos/slot/:slot**
- Retorna todos os produtos de um slot específico
```bash
GET /produtos/slot/PRATELEIRA_A1
```

**3. GET /api/analisar-similares/:produtoId**
- Analisa similares para um produto específico
```bash
GET /api/analisar-similares/1
```

Resposta:
```json
{
  "produto": {
    "id_produto": 1,
    "nome": "Notebook",
    "preco": "450.00",
    "quantidade": 5,
    "slot": "PRATELEIRA_A1"
  },
  "analise": {
    "iguais": [
      {
        "id_produto": 2,
        "nome": "Notebook",
        "preco": "520.00",
        "quantidade": 3,
        "slot": "PRATELEIRA_A1",
        "similaridade": 100
      }
    ],
    "similares": [
      {
        "id_produto": 3,
        "nome": "Notebook Gamer",
        "preco": "850.00",
        "quantidade": 1,
        "slot": "PRATELEIRA_A1",
        "similaridade": 78
      }
    ],
    "totalNoSlot": 3
  }
}
```

**4. POST /produtos**
- Criar novo produto com suporte a slot
```bash
POST /produtos
Content-Type: application/json

{
  "nome": "Teclado Mecânico",
  "preco": 120.50,
  "quantidade": 10,
  "slot": "PRATELEIRA_A1"
}
```

**5. GET /api/relatorio-similares**
- Gera um relatório completo de todos os slots com similares
```bash
GET /api/relatorio-similares
```

Resposta:
```json
[
  {
    "slot": "PRATELEIRA_A1",
    "totalItens": 5,
    "produtosComDuplicatas": 2,
    "detalhes": [
      {
        "produto": "Notebook",
        "similares": [
          {
            "nome": "Notebook Gamer",
            "similaridade": 78
          }
        ]
      }
    ]
  }
]
```

## Instalação / Migração do Banco de Dados

Se estiver usando MySQL:

1. Execute a migração:
```bash
mysql -u root -p sistema_gestao < databasesql/06_add_slot_column.sql
```

Ou execute manualmente:
```sql
ALTER TABLE produto ADD COLUMN slot VARCHAR(50) DEFAULT 'SEM_SLOT';
CREATE INDEX idx_produto_slot ON produto(slot);

CREATE OR REPLACE VIEW v_produtos_por_slot AS
SELECT 
    slot,
    COUNT(*) as total_itens,
    GROUP_CONCAT(DISTINCT nome) as nomes,
    SUM(quantidade) as quantidade_total,
    ROUND(AVG(preco), 2) as preco_medio,
    ROUND(SUM(preco * quantidade), 2) as valor_total
FROM produto
GROUP BY slot
ORDER BY slot;
```

2. Reinicie o servidor backend

## Exemplos Práticos

### Cenário 1: Detecção de Duplicatas Acidentais
```
Slot: ESTOQUE_001
├─ "Mouse Logitech" (10 un, €25.00)
├─ "Mouse Logitech" (5 un, €25.00) ⚠️ IDÊNTICO!
└─ "Mouse Wireless Logitech" (3 un, €28.00) 📌 SIMILAR (85%)
```

### Cenário 2: Produtos Similares com Variações
```
Slot: PRATELEIRA_MONITORS
├─ "Monitor Samsung 24"" (2 un, €180.00)
├─ "Monitor Samsung 27"" (1 un, €250.00) 📌 SIMILAR (80%)
└─ "Monitox Samsung 24 Curvo" (1 un, €220.00) 📌 SIMILAR (78%)
```

## Algoritmo de Similaridade (Levenshtein)

O sistema calcula a distância de edição (número mínimo de operações para transformar uma string em outra):

```
"Notebook" vs "Notebook" = 0 operações = 100% similar ✓
"Notebook" vs "Notebook Dell" = 5 operações = ~85% similar
"Notebook" vs "Netbook" = 3 operações = ~75% similar
"Notebook" vs "Mouse" = 7 operações = ~15% similar ✗
```

## Limitações e Considerações

1. **Sensibilidade**: O limiar de 70% pode ser ajustado no código conforme necessário
2. **Performance**: Para inventários muito grandes (>10.000 produtos), considere indexação adicional
3. **Slot Obrigatório**: Recomenda-se sempre preencer o slot para melhor análise
4. **Case-Insensitive**: Comparações não diferenciam maiúsculas/minúsculas ("MOUSE" = "mouse")

## Melhorias Futuras

- [ ] Permitir ajuste do limiar de similaridade por usuário
- [ ] Sugerir consolidação automática de duplicatas
- [ ] Histórico de similares detectados
- [ ] Relatório periódico enviado por email
- [ ] UI interativa para revisar e atualizar similares

---

**Versão**: 1.0  
**Data de Implementação**: 2026-05-04  
**Desenvolvido para**: Sistema de Gestão de Itens
