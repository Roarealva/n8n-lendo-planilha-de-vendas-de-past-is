# 📊 Assistente de Insights Automático para Vendas com n8n + IA

Simplesmente incrivel, eu era proprietario de uma Pastelaria Mundial Arealva, com uma simples coleta de dados o N8N trata os dados a IA analisa e faz envio das informacoes pelo twilio, voce recebe automaticamente as insights estrategicas.


## Sobre o projeto

Este projeto demonstra a criação de um assistente inteligente capaz de transformar dados simples de vendas em **insights estratégicos automáticos**, utilizando automação de processos, análise de dados e inteligência artificial.

A solução foi desenvolvida pensando em pequenos negócios que possuem dados de vendas, mas muitas vezes não possuem tempo ou conhecimento técnico para analisar informações e tomar decisões baseadas em dados.

O fluxo coleta os dados de vendas de uma planilha, realiza cálculos de desempenho, identifica variações importantes e utiliza inteligência artificial para gerar um resumo executivo enviado automaticamente pelo WhatsApp.

---

## 🚀 Fluxo da automação

```
Planilha Excel de vendas
          ↓
Leitura automática pelo n8n
          ↓
Tratamento e organização dos dados
          ↓
Cálculo de indicadores de vendas
          ↓
Análise com Inteligência Artificial (Gemini)
          ↓
Geração de resumo executivo
          ↓
Envio automático pelo WhatsApp (Twilio)
```

---

## 📈 Como os dados são analisados

A planilha contém informações de vendas organizadas por:

* Semana
* Sabor do produto
* Quantidade vendida
* Valor total vendido

O workflow desenvolvido no n8n realiza uma análise comparativa entre períodos.

A automação:

✅ Identifica a última semana registrada
✅ Compara os resultados com a semana anterior
✅ Calcula a variação percentual de cada produto
✅ Soma o faturamento total do período
✅ Identifica produtos em crescimento ou queda
✅ Envia os dados tratados para uma IA analisar os principais pontos

---

## 🧠 Análise utilizando Inteligência Artificial

Após o processamento dos dados, as informações são enviadas para o modelo Gemini AI com um prompt especializado em análise de negócios.

A IA recebe um resumo estruturado contendo:

* Desempenho individual dos produtos
* Crescimento ou redução das vendas
* Faturamento semanal
* Tendências identificadas

Com base nesses dados, ela gera um relatório curto e objetivo, destacando:

* Melhores desempenhos
* Possíveis problemas
* Tendências que precisam ser acompanhadas

Exemplo de insight gerado:

> "O faturamento total foi de R$ 4.737,50. O sabor Frango destaca-se positivamente com alta de 9,1%, enquanto Queijo apresenta alerta de queda de 3,2%. Vale monitorar a tendência de migração da demanda para sabores salgados tradicionais."

---

## 🛠️ Tecnologias utilizadas

* **n8n** - Automação de workflows e integração entre serviços
* **Google Gemini AI** - Geração automática de insights
* **Twilio WhatsApp API** - Envio das análises para o gestor
* **Excel** - Fonte inicial dos dados
* **JavaScript** - Tratamento e cálculo dos indicadores

---

## 🎯 Aplicação prática

Esse modelo pode ser adaptado para diversos segmentos:

* Restaurantes
* Lanchonetes
* Pequenos varejos
* E-commerces
* Prestadores de serviço

A ideia principal é transformar dados que normalmente ficam parados em planilhas em informações úteis para tomada de decisão.

---

## 💡 Objetivo do projeto

Demonstrar como pequenas empresas podem utilizar automação e inteligência artificial para acompanhar indicadores, identificar oportunidades e tomar decisões mais rápidas sem a necessidade de análises manuais.

Projeto desenvolvido como estudo prático de **Automação, Dados e Inteligência Artificial aplicada aos negócios.**
