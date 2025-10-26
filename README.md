AdIntel 

---

# 🧠 AdIntel Deep Research AI

### Workflow n8n per analisi di brand, ricerca marketing e generazione di idee pubblicitarie multicanale

---

## 📘 Panoramica generale

AdIntel Deep Research AI è un workflow n8n progettato per automatizzare un processo di ricerca e analisi del brand utilizzando AI generativa e fonti web dinamiche.

Riceve in input il nome e il sito web di un’azienda e produce in output:

* Un’analisi dettagliata del brand (mission, USP, tono di voce, competitor, sentiment, keyword)
* Un profilo marketing sintetico (posizionamento, buyer persona, obiettivi, insight)
* Idee creative per Meta Ads, TikTok Ads e Google Ads
* Output finale strutturato in formato JSON, pronto per essere utilizzato in altre automazioni o dashboard.

---

## ⚙️ Architettura del workflow

| Nodo                            | Tipo                                              | Funzione principale                                                                         |
| ------------------------------- | ------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| 🟢 Webhook                  | n8n-nodes-base.webhook                          | Riceve la richiesta HTTP POST contenente companyName e websiteUrl                       |
| 🧩 AdIntel Deep Research AI | @n8n/n8n-nodes-langchain.agent                  | Agente AI principale: orchestra i modelli e i tool                                          |
| 🔵 Google Gemini Chat Model | @n8n/n8n-nodes-langchain.lmChatGoogleGemini     | Modello linguistico LLM di Google per generazione testuale e reasoning strategico           |
| 🧠 Simple Memory            | @n8n/n8n-nodes-langchain.memoryBufferWindow     | Gestione del contesto conversazionale e coerenza tra richieste                              |
| 🔍 Deep Search              | n8n-nodes-base.perplexityTool                   | Effettua ricerche contestuali e analisi dei dati pubblici (siti, social, trend, recensioni) |
| 📦 Structured Output Parser | @n8n/n8n-nodes-langchain.outputParserStructured | Converte l’output AI in JSON validato secondo schema                                        |
| 📨 Respond to Webhook       | n8n-nodes-base.respondToWebhook                 | Invia la risposta JSON finale al client o applicazione chiamante                            |

---

## 🧩 Flusso logico step-by-step

### 1️⃣ Ricezione dei dati (Webhook)

* Endpoint configurato:
  POST /webhook/test
* Input previsto nel body:

  
  {
    "companyName": "Nome Azienda",
    "websiteUrl": "https://www.sitoazienda.com"
  }
  
* Il webhook attiva l’agente AI principale, passando i parametri al flusso.

---

### 2️⃣ Agente AI — “AdIntel Deep Research AI”

Questo nodo è il cuore del sistema.
Utilizza il framework LangChain integrato in n8n per orchestrare:

* il modello linguistico (Google Gemini)
* la memoria conversazionale (Simple Memory)
* il tool esterno (Deep Search via Perplexity)
* e l’output parser strutturato

L’agente riceve un prompt complesso che definisce:

* le 4 fasi operative (Deep Search, Profilo Marketing, Generazione Creativa, Output Finale)
* le regole di scrittura (tono professionale, creativo, linguaggio italiano)
* la struttura dei risultati attesi

---

### 3️⃣ Deep Search (Perplexity Tool)

* Utilizza un modello esterno “sonar-deep-research”
* Effettua ricerche automatiche su:

  * sito web aziendale
  * social media (Instagram, Facebook, LinkedIn, TikTok)
  * recensioni, blog, keyword trend
  * competitor
* Sintetizza i dati in linguaggio naturale per supportare l’AI nella generazione del profilo marketing.


Credenziale richiesta:
🔑 perplexityApi (account Perplexity configurato in n8n)

---

### 4️⃣ Elaborazione AI (Google Gemini)

* Il modello Google Gemini (PaLM API) genera il contenuto sulla base del prompt e delle informazioni estratte.
* Ruolo: elaborare i testi creativi e strategici in linguaggio naturale.
* Credenziale richiesta:
  🔑 Google Gemini (PaLM) API account

---

### 5️⃣ Output strutturato (Structured Output Parser)

* Lo schema JSON garantisce coerenza e leggibilità del risultato.
* Esempio di struttura di output:

  
  {
    "brand_analysis": {...},
    "marketing_profile": {...},
    "creative_assets": {...},
    "deep_search_sources": {...},
    "metadata": {...}
  }
  
* Include esempi concreti per brand (es. EcoStep, Nike) usati come template o test.

---

### 6️⃣ Risposta finale al client (Respond to Webhook)

* Il workflow restituisce il JSON finale come risposta HTTP immediata.
* Output:

  * Contiene le sezioni richieste: analisi, profilo, idee pubblicitarie e meta-informazioni.
  * Pronto per essere salvato o integrato in CRM, dashboard marketing o automazioni successive.

---

## 🧾 Esempio di output finale (estratto)

{
  "brand_analysis": {
    "nome_azienda": "Nike",
    "descrizione_brand": "Nike è un'azienda leader globale...",
    "mission_valori": ["Ispirare e innovare ogni atleta nel mondo"],
    "competitor": ["Adidas", "Puma", "Under Armour"],
    "sentiment_online": "Prevalentemente positivo"
  },
  "marketing_profile": {
    "buyer_persona": {"nome": "L’Atleta Urbano", "eta": "18-35 anni"},
    "obiettivi_pubblicitari": ["Awareness", "Conversion"],
    "tone_creativo": "Audace e motivazionale"
  },
  "creative_assets": {
    "meta_ads": {"hook": "(Video) Senti la differenza?", "cta": "Acquista Ora"},
    "tiktok_ads": {"concept_video": "Challenge fitness con app Nike"},
    "google_ads": {"headline": ["Scarpe Running Nike Uomo"]}
  }
}

---

## 🧠 Memoria e contesto

Il nodo Simple Memory consente all’agente di mantenere il contesto sessione per sessione.
Ogni sessione è identificata da:

sessionKey = $('Webhook').item.json.headers.host

In questo modo, lo stesso host può inviare più richieste mantenendo coerenza nei risultati.

---

## 🔐 Credenziali richieste

| Servizio                 | Tipo credenziale | Descrizione                           |
| ------------------------ | ---------------- | ------------------------------------- |
| Google Gemini (PaLM API) | googlePalmApi  | Accesso al modello LLM di Google      |
| Perplexity AI            | perplexityApi  | Tool di ricerca e analisi Deep Search |

---

## 🧪 Test e debugging

Puoi testare il workflow inviando una richiesta HTTP:

curl -X POST https://<TUO_N8N_DOMINIO>/webhook/test \
  -H "Content-Type: application/json" \
  -d '{
        "companyName": "EcoStep",
        "websiteUrl": "https://www.ecostep.it"
      }'

Riceverai una risposta JSON con l’analisi e le idee creative.

---

## 🚀 Possibili estensioni

* Integrazione con Google Sheets o Airtable per salvare i risultati
* Invio automatico ai team marketing via Slack o Discord
* Pubblicazione del report in formato PDF o Notion
* Aggiunta di un controllo qualità automatico sull’output JSON

---

## 🧩 Requisiti tecnici

* n8n ≥ 1.50
* Plugin LangChain per n8n
* Account validi per:

  * Google Gemini (PaLM API)
  * Perplexity AI

---

## 🏁 Conclusione

AdIntel Deep Research AI è un workflow pronto per l’automazione del marketing intelligence:
integra ricerca, analisi strategica e creatività pubblicitaria in un unico flusso intelligente, scalabile e personalizzabile.

---
