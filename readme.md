# 📊 Dashboard Financeiro - Power BI
## Teste prático para candidatura de vaga

![Power BI](https://github.com/rcaondev/dashboard_test/blob/main/assets/dash.png)


### 🎯 **Objetivo do Projeto**
Dashboard desenvolvido para fornecer insights estratégicos sobre a performance financeira empresarial, focando em análise de receitas, despesas, gestão patrimonial e otimização de fluxo de caixa. O projeto responde a 5 perguntas críticas de negócio através de visualizações intuitivas e métricas calculadas.

---

## 📈 **Principais Insights e Métricas**

### **Performance Financeira Geral**
- **Total de Entradas**: R$ 399.084,36
- **Total de Saídas**: R$ 382.647,88
- **Saldo Líquido**: R$ 16.436,48

### **Performance por Linha de Negócio**
| Grupo | Resultado | % do Total |
|-------|-----------|------------|
| 🏢 Patrimônio | +R$ 204.000 | 52,4% |
| 💰 Receitas | +R$ 124.084 | 31,9% |
| 🏠 Despesas Patrimônio | -R$ 287.126 | -73,7% |
| ⚙️ Despesas Operacionais | -R$ 24.522 | -6,3% |

---

## 🔍 **Estratégia Analítica - 5 Perguntas de Negócio**

### **1. Qual é a Performance Financeira Geral da Empresa?**
**📊 Solução**: Cards de KPIs + Gráfico de barras horizontal
- Visão imediata da saúde financeira
- Comparação direta entre entradas e saídas
- Indicador de rentabilidade líquida

### **2. Como Está a Performance por Linha de Negócio?**
**📊 Solução**: Gráfico de colunas agrupadas por grupo
- Identificação das áreas mais lucrativas
- Análise de consumo de recursos por categoria
- Benchmark entre diferentes linhas de negócio

### **3. Qual é o Desempenho dos Ativos Imobiliários?**
**📊 Solução**: Gráfico de pizza com breakdown por categoria
- Apartamentos (104, 201, 204, 210) representam R$ 346.000 em dividendos
- Loja 06 como ativo complementar
- Análise de rentabilidade por unidade

### **4. Como Está a Distribuição dos Custos Operacionais?**
**📊 Solução**: Gráfico de área empilhada
- Breakdown detalhado por categoria de despesa
- Identificação dos maiores centros de custo
- Oportunidades de otimização

### **5. Qual é o Fluxo de Caixa e Formas de Pagamento?**
**📊 Solução**: Gráfico de linha + Gráfico de rosca
- Evolução temporal do saldo
- Distribuição por meio de pagamento (Boleto 44,27%, PIX 28,89%)
- Gestão de liquidez

---

## 💻 **Códigos DAX Implementado**

### **Medidas Financeiras Principais**

```dax
// Medida: Total de Entradas
Total Entradas = 
SUMX(
    FILTER('Tabela1', 'Tabela1'[Entrada] <> BLANK()), 
    'Tabela1'[Entrada]
)
```

```dax
// Medida: Total de Saídas  
Total Saídas = 
SUMX(
    FILTER('Tabela1', 'Tabela1'[Saída] <> BLANK()), 
    'Tabela1'[Saída]
)
```

```dax
// Medida: Saldo Líquido
Saldo Líquido = [Total Entradas] - [Total Saídas]
```

### **Colunas Calculadas**

```dax
// Coluna: Tipo de Movimentação
Tipo Movimentação = 
IF(
    'Tabela1'[Entrada] <> BLANK(), 
    "Entrada", 
    "Saída"
)
```

```dax
// Coluna: Valor Absoluto
Valor = 
IF(
    'Tabela1'[Entrada] <> BLANK(), 
    'Tabela1'[Entrada], 
    'Tabela1'[Saída]
)
```

### **Medidas de Performance**

```dax
// Medida: Margem Líquida
Margem Líquida = 
DIVIDE(
    [Saldo Líquido], 
    [Total Entradas], 
    0
)
```

```dax
// Medida: Ticket Médio
Ticket Médio = 
DIVIDE(
    [Total Entradas] + [Total Saídas], 
    COUNTROWS('Tabela1')
)
```

```dax
// Medida: Participação por Grupo
% Participação Grupo = 
VAR TotalGrupo = 
    CALCULATE(
        [Total Entradas] + [Total Saídas],
        ALLSELECTED('Tabela1'[Grupo])
    )
RETURN
DIVIDE(
    [Total Entradas] + [Total Saídas],
    TotalGrupo,
    0
)
```

---

## 📊 **Estrutura Técnica**

### **Fonte de Dados**
- **Arquivo**: `TESTE POWERBI_2.xlsx`
- **Registros**: 119 transações
- **Período**: 2024
- **Colunas principais**: Data, Descrição, Grupo, Categoria, Entrada, Saída, Forma de Pagamento

### **Transformações no Power Query**
```m
// Limpeza de dados de Banco
= Table.ReplaceValue(
    #"Previous Step",
    "Banco Teste 1",
    "Banco teste 1",
    Replacer.ReplaceText,
    {"Banco"}
)

// Padronização de representantes
= Table.TransformColumns(
    #"Previous Step",
    {{"Representante", Text.Trim, type text}}
)
```

### **Relacionamentos**
- Modelo estrela com tabela fato principal
- Relacionamentos 1:N com dimensões de tempo
- Filtros bidirecionais habilitados conforme necessário

---

## 📋 **Como Utilizar o Dashboard**

### **Navegação Principal**
1. **Visão Geral**: KPIs e performance consolidada
2. **Análise por Grupo**: Breakdown detalhado por linha de negócio
3. **Gestão Patrimonial**: Foco nos ativos imobiliários
4. **Controle Operacional**: Análise de custos e despesas

### **Filtros Disponíveis**
- 📅 **Período**: Filtro por data/mês
- 🏷️ **Categoria**: Segmentação por tipo
- 🏢 **Grupo**: Análise por linha de negócio
- 💳 **Forma de Pagamento**: Meio utilizado

---

## 🎯 **Resultados e Conclusões**

### **Key Findings**
1. **Saldo positivo** de R$ 16.436,48 indica saúde financeira
2. **Patrimônio** é o maior gerador de valor (52,4% do total)
3. **Despesas patrimoniais** representam o maior custo (73,7%)
4. **Diversificação** adequada de meios de pagamento
5. **Oportunidade** de otimização em despesas operacionais

### **Recomendações Estratégicas**
- 📈 Aumentar receitas de patrimônio através de otimização de aluguéis
- 📉 Revisar despesas patrimoniais para melhorar margem
- ⚖️ Balancear portfolio entre diferentes tipos de ativos
- 💡 Implementar controles mais rígidos em despesas operacionais

---

## 🛠️ **Tecnologias Utilizadas**

- **Microsoft Power BI Desktop**: Desenvolvimento e visualizações
- **DAX (Data Analysis Expressions)**: Cálculos e medidas
- **Power Query**: ETL e transformação de dados
- **Excel**: Fonte de dados original
- **Git**: Controle de versão

---

## 📫 **Contato**

**Desenvolvido por**: [Rodrigo Caon]
**LinkedIn**: [https://www.linkedin.com/in/rodrigo-caon-753370119/]  
**Email**: [rcaondev@gmail.com]  
**GitHub**: [https://github.com/rcaondev]

---

### 🔄 **Versioning**
- **v1.0** - Dashboard inicial com métricas fundamentais
