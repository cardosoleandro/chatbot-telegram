# Bot de Clima no Telegram com n8n

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
