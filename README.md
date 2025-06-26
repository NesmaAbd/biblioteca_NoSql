# Database Biblioteca (MongoDB)

Questa guida spiega come avviare e popolare un database MongoDB per una piccola applicazione di biblioteca usando Docker. È pensata per chi non ha esperienza con database o container.

## Contenuto

* Requisiti
* Installazione
* Avvio del database
* Accesso e verifica
* Struttura del database
* Dati di esempio
* Popolamento del database
* Gestione dei container

---

## Requisiti

* Docker installato sul computer.
* Un editor di testo (facoltativo) come Visual Studio Code.
* Un client grafico per MongoDB, ad esempio DBGate o MongoDB Compass.

---

## Installazione

1. Clona il repository:

```bash
git clone https://github.com/TuoNomeUtente/DatabaseBiblioteca.git
cd DatabaseBiblioteca
```

2. (Facoltativo) Apri la cartella con l'editor di testo per vedere i file.

---

## Avvio del database

Avvia un container MongoDB con il comando seguente:

```bash
docker run --name biblioteca-mongo \
  -p 27017:27017 \
  -d mongo
```

Il database ora ascolta sulla porta 27017.

Per verificare che il container sia attivo:

```bash
docker ps
```

Dovresti vedere una riga con l’immagine **mongo** e il nome **biblioteca-mongo**.

---

## Accesso e verifica

Connettiti al database con DBGate o MongoDB Compass.

* Host: `localhost`
* Porta: `27017`
* Database di lavoro: `biblioteca`

Dopo la connessione puoi vedere le collezioni (se esistono) o crearle.

---

## Struttura del database

Il database è composto da quattro collezioni principali:

| Collezione | Descrizione               |
| ---------- | ------------------------- |
| authors    | Informazioni sugli autori |
| books      | Catalogo dei libri        |
| users      | Utenti della biblioteca   |
| loans      | Prestiti dei libri        |

Le collezioni sono collegate tramite riferimenti (id). Questo evita la duplicazione dei dati e rende più semplici le query.

---

## Dati di esempio

Esempio di documento **books**:

```json
{
  "_id": { "$oid": "6650c0e12bdaf9e3a2c4e123" },
  "title": "Il nome della rosa",
  "authors": [
    { "$oid": "6650c0e12bdaf9e3a2c4e111" }
  ],
  "year": 1980,
  "genre": "Giallo storico"
}
```

Esempio di documento **loans**:

```json
{
  "_id": { "$oid": "6650c0e12bdaf9e3a2c4e222" },
  "user": { "$oid": "6650c0e12bdaf9e3a2c4e200" },
  "book": { "$oid": "6650c0e12bdaf9e3a2c4e123" },
  "borrowed_at": { "$date": "2024-06-01T00:00:00Z" },
  "returned_at": { "$date": "2024-06-15T00:00:00Z" }
}
```

---

## Popolamento del database

Il progetto include uno script **seed.js** che inserisce dati di esempio.

1. Assicurati che il container MongoDB sia in esecuzione.
2. Esegui lo script dal terminale:

```bash
docker exec -i biblioteca-mongo mongosh biblioteca < seed.js
```

Se il file **seed.js** si trova in una cartella diversa, modifica il percorso nel comando.

Dopo l’esecuzione, le collezioni authors, books, users e loans saranno create e popolate.

---

## Gestione dei container

* Fermare il container:

```bash
docker stop biblioteca-mongo
```

* Avviare di nuovo il container:

```bash
docker start biblioteca-mongo
```

* Rimuovere il container definitivamente:

```bash
docker rm -f biblioteca-mongo
```

---

## Conclusione

Hai avviato un database MongoDB di prova per la tua applicazione biblioteca e caricato dati di esempio. Puoi ora collegare un backend o sperimentare con query e aggregazioni.
