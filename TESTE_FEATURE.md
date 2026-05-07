<!-- Exemplo de uso da feature de análise de similares -->

# 🧪 Teste Rápido - Feature de Análise de Similares

## Passo 1: Adicionar Produtos de Teste

Abra o aplicativo e adicione os seguintes produtos no mesmo slot para testar:

### Teste 1: Detecção de IDÊNTICOS
1. **Produto 1**
   - Nome: `Notebook Dell Inspiron 15`
   - Preço: `€450.00`
   - Quantidade: `2`
   - Slot: `PRATELEIRA_A1`
   - ➕ Adicionar

2. **Produto 2**
   - Nome: `Notebook Dell Inspiron 15`
   - Preço: `€450.00`
   - Quantidade: `1`
   - Slot: `PRATELEIRA_A1`
   - ➕ Adicionar
   
   **Resultado Esperado**: ⚠️ Aviso IDÊNTICO aparece no card do Produto 2

---

### Teste 2: Detecção de SIMILARES
3. **Produto 3**
   - Nome: `Teclado Mecânico RGB`
   - Preço: `€120.00`
   - Quantidade: `5`
   - Slot: `CAIXA_PERIFERICOS`
   - ➕ Adicionar

4. **Produto 4**
   - Nome: `Teclado RGB Sem Fio`
   - Preço: `€110.00`
   - Quantidade: `3`
   - Slot: `CAIXA_PERIFERICOS`
   - ➕ Adicionar

   **Resultado Esperado**: 📌 Aviso SIMILARES com ~80% de similaridade

---

### Teste 3: Sem Similares (Slot Diferente)
5. **Produto 5**
   - Nome: `Mouse Logitech`
   - Preço: `€30.00`
   - Quantidade: `10`
   - Slot: `CAIXA_PERIFERICOS`
   - ➕ Adicionar

6. **Produto 6**
   - Nome: `Mouse Logitech`
   - Preço: `€30.00`
   - Quantidade: `7`
   - Slot: `GAVETA_02`
   - ➕ Adicionar

   **Resultado Esperado**: ❌ Sem avisos (slots diferentes)

---

## Passo 2: Verificar Análise Visual

Após adicionar os produtos, verifique:
- ✓ Cada card mostra `📍 Slot: VALOR`
- ✓ Produtos com nomes iguais mostram `⚠️ IDÊNTICOS`
- ✓ Produtos com nomes parecidos mostram `📌 SIMILARES` com percentual
- ✓ Clique em "Dar Baixa" ou "Remover" funciona normalmente

---

## Passo 3: Testar API Backend (Opcional)

Se estiver usando o servidor Node.js:

```bash
# 1. Listar todos os produtos
curl http://localhost:3001/produtos

# 2. Listar produtos de um slot específico
curl http://localhost:3001/produtos/slot/PRATELEIRA_A1

# 3. Analisar similares de um produto (substitua ID)
curl http://localhost:3001/api/analisar-similares/1

# 4. Obter relatório geral
curl http://localhost:3001/api/relatorio-similares
```

---

## Esperado vs Atual

### ✅ Funcionamentos Esperados
- [x] Campo de Slot no formulário
- [x] Slot salvo nos produtos
- [x] Detecção de nomes iguais
- [x] Detecção de nomes similares (70%+)
- [x] Avisos visuais com cores
- [x] Informações de similares no card
- [x] API endpoints funcionando
- [x] localStorage preservando slots

### 🔍 Casos Especiais
- [x] Produtos com mesmo nome mas slots diferentes = SEM aviso
- [x] Produto único no slot = SEM aviso
- [x] Nomes com variações pequenas (ex: "Mouse" vs "Mause") = SIMILAR ~75%
- [x] Case-insensitive ("MOUSE" vs "mouse" = 100% similar)

---

## Ajustes Possíveis

Se quiser alterar o comportamento:

1. **Mudar limiar de similaridade de 70% para 80%**:
   - No `index.html`, procure: `if (similaridade >= 70)`
   - Altere para: `if (similaridade >= 80)`

2. **Mostrar avisos para >50% de similaridade**:
   - Altere o mesmo valor para: `if (similaridade >= 50)`

3. **Mudança no backend**:
   - Em `server.js`, procure o mesmo padrão e altere

---

**Pronto para testar! 🚀**
