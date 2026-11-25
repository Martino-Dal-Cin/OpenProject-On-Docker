# OpenProject Docker

Configurazione per avviare l'immagine ufficiale OpenProject 17 con Docker Compose, in un singolo container.

## Requisiti

- Docker Desktop (Windows) oppure Docker Engine e Docker Compose
- Porta locale `8081` libera
- RAM sufficiente per OpenProject (consigliati almeno 4 GB disponibili a Docker)

## Avvio rapido (Windows PowerShell)

1. Clona o scarica questo repository.
2. Apri PowerShell nella cartella del progetto.
3. Crea il file locale delle variabili:

   ```powershell
   Copy-Item .env.example .env
   ```

4. Genera una chiave:

   ```powershell
   $key = -join ((1..64) | ForEach-Object { '{0:x2}' -f (Get-Random -Maximum 256) })
   notepad .env
   ```

   Sostituisci `REPLACE_WITH_A_RANDOM_SECRET` in `.env` con la chiave generata. Salva il file. Non pubblicare `.env`.

5. Avvia in background:

   ```powershell
   docker compose up -d
   ```

6. Controlla lo stato e i log:

   ```powershell
   docker compose ps
   docker compose logs -f openproject
   ```

7. Apri <http://127.0.0.1:8081>.

Al primo avvio l'inizializzazione può richiedere alcuni minuti.

## Credenziali iniziali

Per una nuova installazione, le credenziali iniziali standard sono:

- Utente: `admin`
- Password: `admin`

Al primo accesso cambia la password.

Una volta entrato nell'account amministratore è possibile abilitare l'attivazione automatica per tutti gli utenti che si registrano.
Vai sull'icona profilo dell'ammnistratore in alto a destra, click su Amministrazione, poi Autenticazione, Accesso e registrazione, Registrazione --> Registrazione Automatica: Imposta Attivazione account automatica

## Comandi utili

```powershell
# Avvia in background
docker compose up -d

# Ferma senza cancellare i dati
docker compose stop

# Riavvia
docker compose restart

# Stato
docker compose ps

# Log
docker compose logs -f openproject

# Aggiorna l'immagine e ricrea il container
docker compose pull
docker compose up -d
```

## Volumi e persistenza

Compose crea tre volumi Docker nominati:

- `openproject_data`: directory dati principale
- `openproject_pgdata`: dati PostgreSQL
- `openproject_assets`: allegati e asset caricati

I volumi restano anche se il container viene rimosso. Non usare `docker compose down -v` se vuoi conservare i dati.

## Backup

Prima di aggiornare o spostare l'installazione, esegui un backup coerente di database e allegati. Non copiare i file PostgreSQL mentre il database è in esecuzione. Consulta la guida ufficiale:
<https://www.openproject.org/docs/installation-and-operations/operation/backing-up/>

## Sicurezza

- `.env` contiene il segreto dell'istanza e non deve essere committato.
- La porta è pubblicata su `127.0.0.1`: l'applicazione è accessibile solo dal PC locale.
- Per esporre il servizio in rete o su Internet servono HTTPS, un hostname adeguato e ulteriori misure di sicurezza.
- Cambia la password amministratore al primo accesso.

## Nota

Questo repository contiene la configurazione Docker per distribuire l'immagine `openproject/openproject:17`. Non contiene il codice sorgente di OpenProject.
