# Automação de Testes API PTAX - Banco Central (Postman)

## 🎯 Objetivo
Este projeto contém duas automações de teste desenvolvidas no **Postman** para validar a API PTAX do Banco Central do Brasil.
As validações garantem:

* **Disponibilidade** do serviço.
* **Estrutura correta** da resposta.
* **Integridade** dos valores monetários.
* **Consistência de tendência** no período consultado.

> **Nota:** O formato de resposta utilizado nestes testes é `text/plain` (CSV).

---

## 🚀 Automação 1 – Cotação do Dólar por Dia

### **Endpoint**
`GET https://olinda.bcb.gov.br/olinda/servico/PTAX/versao/v1/odata/CotacaoDolarDia(dataCotacao=@dataCotacao)?@dataCotacao='MM-DD-YYYY'&$format=text/plain`

**Exemplo:** `@dataCotacao='10-22-2025'`

### **O que é validado**
1. **Status da Requisição:** Deve retornar `HTTP 200`.
2. **Estrutura da Resposta:**
    * O corpo não pode estar vazio.
    * Não pode conter apenas o cabeçalho.
    * Deve possuir pelo menos 1 linha de cabeçalho e 1 linha de dados.
3. **Validação dos Valores:**
    * **cotacaoCompra:** Não pode estar vazio, deve ser numérico e maior que zero.
    * **cotacaoVenda:** Não pode estar vazio, deve ser numérico e maior que zero.

---

## 📈 Automação 2 – Cotação do Dólar por Período

### **Endpoint**
`GET https://olinda.bcb.gov.br/olinda/servico/PTAX/versao/v1/odata/CotacaoDolarPeriodo(dataInicial=@dataInicial,dataFinalCotacao=@dataFinalCotacao)?@dataInicial='MM-DD-YYYY'&@dataFinalCotacao='MM-DD-YYYY'&$format=text/plain`

**Exemplo:** `@dataInicial='10-15-2019'&@dataFinalCotacao='10-22-2019'`

### **O que é validado**
1. **Status da Requisição:** Deve retornar `HTTP 200`.
2. **Estrutura da Resposta:**
    * Deve conter múltiplos registros.
    * Deve possuir pelo menos 1 cabeçalho e 2 ou mais linhas de dados.
3. **Validação por Linha:**
    * Para cada registro retornado: o valor de venda deve ser numérico e maior ou igual a zero.
4. **Validação de Tendência:**
    * Compara o primeiro e o último valor do período.
    * Identifica: **Alta** (último > primeiro), **Queda** (último < primeiro) ou **Estabilidade**.

---

## 🛠️ Como Executar

### **Passos**
1. Importar a requisição no **Postman**.
2. Inserir o script de validação na aba **Tests**.
3. Ajustar as datas nos parâmetros da URL conforme necessário.
4. Executar a requisição (**Send**).
5. Verificar os resultados na aba **Test Results**.
