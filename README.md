# AdIntel
 Workflow n8n per analisi di brand, ricerca marketing e generazione di idee pubblicitarie multicanale

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

Vuoi che ora ti generi anche un file `README.md` completo pronto da copiare su GitHub (con struttura, badge e tabella di configurazione dei nodi)?
Posso anche includere una sezione di installazione passo-passo per utenti n8n.
