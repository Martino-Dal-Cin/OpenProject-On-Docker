# Risoluzione problemi

## La pagina non risponde

Controlla lo stato e i log:

```powershell
docker compose ps
docker compose logs --tail=200 openproject
curl.exe -v http://127.0.0.1:8081
```

Verifica che la porta `8081` non sia già utilizzata.

## Il container si arresta

```powershell
docker compose ps -a
docker compose logs --tail=200 openproject
```

## Conservazione dei dati

Evita `docker compose down -v` e `docker volume prune` se vuoi conservare database e allegati.
