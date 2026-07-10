# Assistente de Insights Automático — Vendas de Pastéis

Projeto de estudo que automatiza a análise de vendas: lê uma planilha de
vendas semanal, usa uma IA (Google Gemini, gratuito) para gerar um resumo
com variações, alertas e tendências em português, e envia esse resumo
automaticamente por **WhatsApp** toda semana — usando **n8n** como
orquestrador.

## O que o projeto contém

| Arquivo | O que é |
|---|---|
| `vendas_pasteis.xlsx` | Planilha de exemplo com 8 semanas de vendas, 5 sabores |
| `assistente-insights-workflow.json` | Fluxo pronto para importar no n8n |

## Como tudo funciona (visão geral)

```
Disparo semanal (toda segunda, 8h)
        ↓
Lê a planilha de vendas (Excel)
        ↓
Calcula totais e variação de cada sabor (semana atual vs. anterior)
        ↓
Envia os números pra IA (Gemini), que escreve o resumo em português
        ↓
Formata a mensagem final
        ↓
Envia por WhatsApp via Twilio
```

Nenhuma dessas etapas é feita manualmente depois de configurado — o n8n
dispara sozinho toda semana no horário definido.

## O que é o n8n, rapidamente

n8n é uma ferramenta de automação **visual** (parecida com o que o
Zapier faz, mas open-source e mais flexível). Você monta o fluxo
conectando "caixinhas" (nós), cada uma fazendo uma tarefa — sem
precisar escrever um programa do zero. Como você nunca usou, o roteiro
abaixo assume conhecimento zero.

## Passo a passo completo

### 1. Instalar o n8n localmente

Você já tem Node.js instalado (se seguiu os passos do Claude Code
anteriormente). No terminal:

```bash
npx n8n
```

Isso baixa e roda o n8n na primeira vez (pode demorar um pouco). Depois
de iniciar, abra no navegador:

```
http://localhost:5678
```

Na primeira vez, ele pede pra você criar um usuário local (e-mail e
senha só pra você, fica salvo no seu computador).

### 2. Importar o fluxo pronto

1. Dentro do n8n, clique em **"Add workflow"** (ou nos três pontinhos
   no canto → **"Import from File"**)
2. Selecione o arquivo `assistente-insights-workflow.json` deste
   repositório
3. O fluxo completo (8 caixinhas conectadas) vai aparecer na tela

### 3. Ajustar o caminho da planilha

Clique no nó **"Ler Planilha (arquivo local)"** e troque o campo de
caminho do arquivo pelo caminho real onde você salvou o
`vendas_pasteis.xlsx` no seu computador (ex:
`C:\Users\SeuNome\Documents\vendas_pasteis.xlsx` no Windows, ou
`/home/seunome/vendas_pasteis.xlsx` no Mac/Linux).

### 4. Criar a credencial de IA (Google AI Studio — gratuito)

Pra esse projeto usamos o **Google Gemini**, via Google AI Studio, que
tem camada gratuita sem necessidade de cartão de crédito — ideal pra
quem está estudando.

1. Acesse [aistudio.google.com](https://aistudio.google.com) e faça
   login com sua conta Google
2. Clique em **"Get API Key"** → **"Create API Key"**
3. Copie a chave gerada (confira que não veio nenhum espaço em branco
   colado no início ou no fim)
4. No n8n, clique no nó **"Gerar Insight com IA (Gemini)"**
5. No campo "Authentication", deixe **"Generic Credential Type"**
6. Em "Generic Auth Type", deixe **"Query Auth"**
7. Clique em **"Create New Credential"**
8. Preencha:
   - **Name**: `key`
   - **Value**: cole sua chave da API aqui
9. Salve

> Usamos o tipo "Query Auth" em vez da credencial pronta do Google,
> porque a credencial pronta às vezes falha no teste automático de
> conexão do n8n mesmo com a chave certa. O "Query Auth" evita esse
> teste e funciona de forma mais confiável.

Isso guarda sua chave de forma segura dentro do n8n — ela **não** vai
junto quando você exporta ou publica o fluxo no GitHub.

> **Sobre o limite gratuito**: a camada free do Google AI Studio tem um
> número de requisições por dia mais que suficiente pra rodar esse
> fluxo uma vez por semana. Não é ilimitado, mas pra estudo e uso
> pessoal como esse, não deve gerar nenhuma cobrança.

### 5. Configurar o WhatsApp (via Twilio Sandbox — gratuito para testes)

O WhatsApp não permite automação direta sem um provedor autorizado. A
forma mais simples e gratuita para aprender é o **Twilio WhatsApp
Sandbox**:

1. Crie uma conta gratuita em [twilio.com](https://www.twilio.com)
2. No painel, vá em **Messaging → Try it out → Send a WhatsApp message**
3. Você vai receber um número Twilio de teste (ex: `+1 415 523 8886`) e
   um código tipo `join palavra-chave`
4. Pelo **seu WhatsApp pessoal**, envie esse código pro número Twilio —
   isso "conecta" seu número à sandbox (validade de 72h, depois precisa
   reenviar o código)
5. Copie seu **Account SID** e **Auth Token** (aparecem no painel
   principal do Twilio)
6. No n8n, clique no nó **"Enviar por WhatsApp (Twilio)"** → crie uma
   credencial do tipo **Twilio API** com esses dados
7. No mesmo nó, troque `whatsapp:+55SEUNUMEROAQUI` pelo seu número de
   WhatsApp real, no formato `whatsapp:+55DDDNUMERO`

> Isso é um ambiente de **teste**. Pra usar em produção (enviar pra
> qualquer número, sem o passo de "join"), seria necessário migrar
> pra WhatsApp Business API oficial da Meta — processo mais burocrático,
> não necessário para fins de aprendizado.

### 6. Testar

1. Clique no nó **"Disparo Manual (para testar)"**
2. Clique em **"Execute Workflow"** (ou no botão de play daquele nó)
3. Acompanhe cada caixinha rodando (fica verde quando dá certo)
4. Em poucos segundos, a mensagem deve chegar no seu WhatsApp

### 7. Ativar a automação semanal

Depois de testar e funcionar, clique no botão **"Active"** no topo do
fluxo. A partir daí, ele roda sozinho toda segunda-feira às 8h, sem
você precisar abrir o n8n.

## Sobre segurança (importante antes de publicar no GitHub)

- O arquivo `assistente-insights-workflow.json` **não contém** suas
  chaves de API nem tokens — o n8n guarda credenciais separadamente,
  fora do arquivo exportado. Pode publicar sem medo nesse ponto.
- Ainda assim, revise o arquivo antes de subir pra confirmar que você
  não colou nenhuma chave direto num campo de texto por engano.
- O número de WhatsApp de destino (`whatsapp:+55SEUNUMEROAQUI`) fica
  como um placeholder genérico no arquivo — assim você não expõe seu
  número pessoal ao publicar.

## Personalizando

- **Trocar os dados**: edite `vendas_pasteis.xlsx` com dados reais de
  vendas, mantendo as colunas `semana`, `data_inicio`, `sabor`,
  `quantidade`, `valor_unitario`, `valor_total`.
- **Mudar o dia/horário do disparo**: no nó "Disparo Semanal", ajuste
  o dia da semana e a hora.
- **Trocar WhatsApp por e-mail**: substitua o nó final do Twilio por
  um nó de e-mail (Gmail/SMTP), já disponível nativamente no n8n.

## Próximos passos possíveis

- Conectar direto numa planilha do Google Sheets em vez de arquivo
  local (permite atualizar os dados de qualquer lugar)
- Adicionar um gráfico gerado automaticamente junto com o texto
- Comparar múltiplas semanas de uma vez, não só a última vs. anterior
