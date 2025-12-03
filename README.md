# **Trello*

Un Trello-clone open-source: backend e frontend per una piattaforma Kanban collaborativa in tempo reale.

**Versione:** 1.0
**Data:** 19/11/2025

---

# **Descrizione**

Trellino è un progetto didattico/prototipale che implementa le funzionalità fondamentali di una piattaforma Kanban moderna: board, liste, card, drag-and-drop, collaborazione live, autenticazione e permessi.

Il repository contiene:

* **back-end/** — server API (REST + WebSocket), logica di business, ordering, modelli dati.
* **front-end/** — interfaccia web con aggiornamenti in tempo reale.
* **docs/** — documentazione funzionale, tecnica e analisi architetturali.

---

# **Caratteristiche principali**

* Autenticazione utente (JWT)
* CRUD completo: workspace, board, liste, card
* Drag & Drop in tempo reale tramite WebSocket
* Ordering stabile con indici decimali
* Permessi per board e workspace (viewer, member, admin)
* Commenti, checklist, attività card
* Ricerca card
* Struttura scalabile per ambienti multi-istanza (Redis Pub/Sub)

---

# **Architettura (breve)**

Il backend è diviso in componenti indipendenti:

* **Gateway / API Server**
  Gestisce autenticazione, API REST, permessi e validazione input.

* **Board & Card Service**
  Si occupa di workspace, board, liste, card, ordering e transazioni atomiche.

* **Real-time Server**
  Gestisce WebSocket, sincronizzazione modelli e broadcast degli eventi board.

* **Redis Pub/Sub**
  Garantisce che più istanze del server si aggiornino tra loro.

Per i dettagli completi vedere:
`docs/backend/analisi-tecnica.md`

---

# **Tecnologie suggerite**

* **Linguaggi:** Node.js / TypeScript oppure Python (FastAPI)
* **Database:** PostgreSQL
* **Caching & Pub/Sub:** Redis
* **WebSocket:** socket.io, ws, o soluzioni native (a seconda dell’implementazione)
* **Container:** Docker (Kubernetes opzionale)

---

# **Requisiti**

* Node.js ≥ 18 (se backend Node)
* PostgreSQL 13+
* Redis 6+

---

# **Setup rapido (sviluppo locale)**

Apri un terminale PowerShell:

```bash
# Clona il repository
git clone https://github.com/.../trellino.git
cd trellino
```

### Avvio con Docker (se fornito)

```bash
# docker-compose up --build
```

### Oppure: avvio manuale

**Backend**

```bash
cd back-end
npm install
npm run dev
```

**Frontend**

```bash
cd ../front-end
npm install
npm run dev
```

Adatta i comandi alla tecnologia scelta (Node, Python, ecc.).

---

# **Variabili d’ambiente comuni**

* **DATABASE_URL** — connessione PostgreSQL
* **REDIS_URL** — connessione Redis
* **JWT_SECRET** — segreto per i token
* **PORT** — porta API
* **WS_PORT** — porta WebSocket (se separata)

Esempio `.env`:

```
DATABASE_URL=postgres://user:password@localhost:5432/trellino
REDIS_URL=redis://localhost:6379
JWT_SECRET=changeme
PORT=4000
```

---

# **Esecuzione locale**

* Backend:
  `cd back-end && npm run dev`

* Frontend:
  `cd front-end && npm run dev`

* Documentazione tecnica:
  Apri `docs/` o `docs/backend/analisi-tecnica.md`

---

# **API (breve panoramica)**

Endpoint principali:

### **Autenticazione**

* `POST /auth/register`
* `POST /auth/login`

### **Workspace**

* `GET /workspaces`
* `POST /workspaces`

### **Board**

* `POST /boards`
* `GET /boards/:id`

### **Liste**

* `POST /boards/:id/lists`
* `PATCH /lists/:id`

### **Card**

* `POST /lists/:id/cards`
* `PATCH /cards/:id`
* `PATCH /cards/:id/move` (drag & drop)

### **Commenti & Checklist**

* `POST /cards/:id/comments`
* `POST /cards/:id/checklist`

Per la lista completa:
documentazione API → `docs/backend/api/`

---

# **WebSocket / Real-time**

Pattern consigliato:

* Canale WS per ogni board
* Redis Pub/Sub per sincronizzare più istanze
* Eventi principali:

  * `card:moved`
  * `card:updated`
  * `list:updated`
  * `board:sync`
  * `presence:change`

La sincronizzazione è progettata per evitare conflitti durante movimenti simultanei.

---

# **Deploy**

Approccio consigliato:

1. Containerizzare frontend e backend (Docker)
2. Utilizzare servizi gestiti per PostgreSQL e Redis
3. Deploy su AWS/GCP/Azure o piattaforme PaaS (Render, Railway, Fly.io)
4. Configurare load balancer + scaling orizzontale
5. Abilitare health checks e readiness probes

---

# **Contribuire**

Per contribuire:

1. Fai il fork del repository
2. Crea una branch dedicata
3. Implementa la feature/fix
4. Apri una Pull Request chiara
5. Aggiungi test quando necessario

Controlla anche **CONTRIBUTING.md** se disponibile.

---

# **Licenza**

Il progetto è rilasciato sotto la licenza riportata nel file `LICENSE`.

---

# **Contatti**

Per domande o proposte:
Apri una *issue* nel repository o contatta il maintainer.
