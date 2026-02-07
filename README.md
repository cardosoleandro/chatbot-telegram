# ⚡️ Bot de Clima no Telegram com n8n

## 📌 Visão Geral do Projeto

Este projeto consiste no desenvolvimento de um bot de clima no Telegram, criado como atividade prática da Fase 2 do desafio da pós-graduação, utilizando a ferramenta n8n (versão 1.106.3) para orquestração de automações.

O bot permite que usuários consultem informações climáticas de uma cidade específica diretamente pelo Telegram, integrando:
- Telegram Bot API (entrada e saída)
- OpenWeatherMap API 2.5 (dados climáticos)
- Google Gemini (formatação de mensagens e tratamento de erro)

**Todo o fluxo foi construído utilizando nodes nativos do n8n e o sistema padrão de credenciais da plataforma.**



## 🤖 Como acessar e conversar com o bot

Para testar o bot na prática, siga os passos abaixo:
1.	Acesse o bot no Telegram pelo link:
👉 https://t.me/OroroMunroe_bot
2.	Abra a conversa com o bot
3.	Envie uma mensagem informando a cidade e o país no formato:
    ```Cidade, País```

**Exemplos válidos:**
  - Ipatinga, BR
  - São Paulo, BR
  - Rio de Janeiro, BR
  - New York, US

**📌 Observação:**
O nome do bot @OroroMunroe_bot é uma referência à personagem Tempestade (Ororo Munroe) dos X-Men, associada ao controle do clima.

Ao enviar a mensagem, o bot processará a solicitação e responderá com as informações climáticas da cidade informada.



## 🔧 Ferramentas e Tecnologias Utilizadas
	•	n8n – versão 1.106.3
	•	Telegram Bot API
	•	OpenWeatherMap API (versão 2.5)
	•	Google Gemini (via LangChain node do n8n)


## 🔐 Credenciais

Todas as credenciais utilizadas no projeto foram criadas e gerenciadas utilizando o sistema padrão de criação de credenciais do n8n, conforme orientado no desafio.

Credenciais utilizadas:
- Telegram API
- OpenWeatherMap API
- Google Gemini (Google API)

Nenhuma credencial sensível foi inserida diretamente em código ou variáveis hardcoded.


## 🧱 Arquitetura e Fluxo da Automação

O fluxo segue a seguinte lógica:
1. Receber a mensagem do usuário via Telegram
2. Normalizar a entrada (cidade e país)
3. Consultar a API OpenWeatherMap
4. Processar os dados retornados
5. Gerar uma resposta amigável
6. Tratar erros de forma orientativa


## 🔁 Descrição dos Nodes

### 1️⃣ Telegram Trigger
	•	Função: Captura mensagens enviadas ao bot no Telegram.
	•	Evento monitorado: message
	•	Saída: Texto enviado pelo usuário.


### 2️⃣ Edit Fields (Normalização da Entrada)
	•	Função: Preparar a entrada do usuário para consulta na API.
	•	Regras aplicadas:
	•	Conversão para lower case
	•	Separação entre cidade e país usando vírgula
	•	Remoção de espaços extras

**Exemplo:**
- Entrada: Ipatinga, BR → Saída: ipatinga,br
- Entrada: Timóteo, BR → Saída: timóteo,br
- Entrada: São Paulo, BR → Saída: são paulo,br

Essa normalização garante maior consistência nas consultas.


### 3️⃣ OpenWeatherMap
	•	Função: Consulta os dados climáticos da cidade informada.
	•	Configuração:
	•	cityName: valor normalizado
	•	format: Metric (graus celsius)
	•	language: pt_BR

📌 O node nativo do OpenWeatherMap entende e realiza automaticamente o encoding da vírgula para a URL.


### 4️⃣ Edit Fields (Processamento da Resposta)
	•	Função: Extrair dados relevantes da resposta da API.
	•	Dados tratados:
	•	Temperatura (arredondada)
	•	Nome da cidade
	•	País
	•	Descrição do clima


### 5️⃣ parseMessage (Google Gemini)
	•	Função: Converter os dados técnicos em uma mensagem natural.
	•	Características da resposta:
	•	Idioma: português (pt_BR)
	•	Tom amigável e conversacional
	•	Objetividade
	•	Uso obrigatório do formato XXºC


### 6️⃣ Send a Text Message (Resposta ao Usuário)
	•	Função: Enviar a resposta final para o usuário no Telegram.

Exemplo:
    ```
    Agora em Ipatinga está nublado e a temperatura é 26°C.
    ```


## ⚠️ Tratamento de Erros

Caso a cidade não seja encontrada ou o formato informado esteja incorreto, o fluxo segue um caminho alternativo:
- Um modelo de linguagem (Google Gemini) gera uma resposta curta e clara
- O erro técnico não é exposto ao usuário
- O usuário recebe orientação de como corrigir a entrada

  **Exemplo de resposta:**
    ```
    ❌ Cidade não encontrada. Use o formato Cidade, país (ex.: São Paulo, BR).
    ```

Esse tratamento melhora a experiência do usuário e evita mensagens confusas.


## ✅ Conclusão

O bot de clima desenvolvido está funcional, testável e documentado de forma clara, cumprindo todos os requisitos propostos no desafio da Fase 2, demonstrando:
- Uso correto do n8n como ferramenta de automação
- Integração com APIs externas
- Normalização e validação de dados de entrada
- Tratamento de erros
- Comunicação clara com o usuário final

🚀 Como importar, configurar e executar o Bot de Clima

Este projeto utiliza o n8n (versão 1.106.3) para criar um bot de clima no Telegram, integrando a API OpenWeatherMap e, opcionalmente, Google Gemini para respostas mais naturais.

⸻

# 📦 Importando o workflow no n8n
	1.	Acesse o painel do seu n8n
	2.	No menu lateral, clique em Workflows
	3.	Clique em Import
	4.	Selecione Import from File
	5.	Faça upload do arquivo JSON do workflow disponível neste repositório
	6.	Após a importação, o workflow aparecerá na lista de workflows


## 🔐 Configuração das credenciais

O workflow pode funcionar com credenciais obrigatórias e credenciais opcionais.

Credenciais obrigatórias
- Telegram Bot API
- OpenWeatherMap API

Credencial opcional
- Google Gemini (Google API)

**Todas as credenciais devem ser criadas usando o sistema padrão de credenciais do n8n.**



### 🤖 Telegram Bot API (obrigatória)

Criando o bot no Telegram
1. No Telegram, abra uma conversa com @BotFather
2. Envie:
	```
	/start
	```
3. Em seguida:
	```
	/newbot
	```
4. Defina um nome e um username para o bot
5. Ao final, o BotFather fornecerá o token do bot

Esse token corresponde à variável:
**TELEGRAM_BOT_TOKEN**


**Criando a credencial no n8n**
1. No n8n, vá em Credentials
2. Clique em Add Credential
3. Selecione Telegram API
4. No campo Access Token, cole o token gerado pelo BotFather
5. Salve a credencial


### 🌦️ OpenWeatherMap API (obrigatória)

Gerando a API Key
1. Acesse: https://openweathermap.org/
2. Crie uma conta (caso ainda não tenha)
3. Clique sobre o seu usuário e vá até a seção API Keys
4. Gere uma nova chave

Essa chave corresponde à variável:
**OPENWEATHER_API_KEY**


**Criando a credencial no n8n**
1. No n8n, vá em Credentials
2. Clique em Add Credential
3. Selecione OpenWeatherMap API
4. No campo API Key, cole a chave gerada
5. Salve a credencial


### 🤖 Google Gemini API (opcional)

**⚠️ Esta credencial é opcional.**
O workflow funciona normalmente sem o Gemini, retornando mensagens padrão.
Quando configurado, o Gemini é utilizado apenas para melhorar a naturalidade das respostas e mensagens de erro.

Gerando a API Key
1. Acesse: https://ai.google.dev/
2. Crie um projeto (caso necessário)
3. Gere uma chave de API para o Gemini

Essa chave corresponde à variável:
**GEMINI_API_KEY**


**Criando a credencial no n8n**
1. No n8n, vá em Credentials
2. Clique em Add Credential
3. Selecione Google Palm API
4. Cole a chave de API do Gemini
5. Salve a credencial

**📌 Caso essa credencial não seja criada, o fluxo continua funcionando, pois os nodes de IA possuem tratamento de erro configurado.**


## 🔗 Associando as credenciais ao workflow
	1.	Abra o workflow importado
	2.	Abra todos os nodes dos tipos:
		•	Telegram Trigger
		•	Telegram Send a text message
		•	OpenWeatherMap
		•	Nodes Gemini (opcional)
	3.	Selecione as credenciais correspondentes criadas anteriormente
	4.	Salve o workflow


## ▶️ Ativando o workflow
	1.	Com o workflow aberto, clique em Activate
	2.	O webhook do Telegram será registrado automaticamente
	3.	O bot estará pronto para receber mensagens


## 🧪 Como testar o chatbot

Acessando o bot no Telegram

👉 Envie uma mensagem ao bot que você acabou de criar.
Alternativa: O bot criado para construção desse fluxo continua ativo e pode ser testado em https://t.me/OroroMunroe_bot, basta enviar uma mensagem para ele.
