# SQL töö - kuidas andmebaas DataGripi külge saada

Selles juhendis on kõik sammud alates Dockeri käivitamisest kuni töö Teamsi esitamiseni.
Kui midagi ei tööta, loe veateadet - seal on peaaegu alati kirjas, mis viga on.

---

## 1. Käivita Docker

Klooni oma klassi repositoorium ja käivita see:

```bash
git clone https://github.com/Kasparsu/ta24blearndb2
cd ta24blearndb2
docker compose up -d
```

Kontrolli, et konteinerid käivad:

```bash
docker compose ps
```

`db` teenus peab olema `running`. Kui ei ole, siis DataGrip ühendust ei saa - kõik
järgnev eeldab, et see töötab.

Andmebaasi ühendusandmed tulevad `docker-compose.yml`-ist:

| Väli | Väärtus |
|---|---|
| Host | `localhost` |
| Port | `33061` |
| Kasutaja | `root` |
| Parool | `example` |
| Andmebaas | **jäta tühjaks** |

> Port on **33061**, mitte 3306. `docker-compose.yml`-is on rida `- 33061:3306`:
> vasak pool on sinu arvuti port, parem pool konteineri oma. DataGrip ühendub
> sinu arvuti pordiga.
>
> Andmebaasi väli jääb tühjaks, sest `docker-compose.yml` ei loo ühtegi andmebaasi -
> selle teed ise 3. sammus.

---

## 2. Loo ühendus DataGripis (esimesel korral)

1. **File | New | Data Source** ja vali **MariaDB**.
2. Täida Host, Port, User, Password ülaltoodud tabeli järgi.
3. **Lae draiverid alla.** Akna all servas on link
   **Download missing driver files** - kliki see ära. Ilma draiverita ühendust ei tule
   ja DataGrip annab vea. Seda on vaja teha ainult üks kord.
4. **Test Connection** (samuti akna all servas). Oota, kuni tuleb roheline kinnitus koos
   serveri versiooniga.
5. Alles siis vajuta **OK**.

> Tee **alati Test Connection enne OK-d**. Kui vajutad kohe OK ja ühendus on katki, siis
> näed vigu alles hiljem päringuid tehes ja arvad, et viga on SQL-is.

Kui Test Connection ebaõnnestub:
- käib `docker compose ps` järgi `db` konteiner?
- kas port on kindlasti `33061`?
- kas parool on `example`?

---

## 3. Loo skeem

Uues ühenduses ei ole veel ühtegi andmebaasi. Tee see ise:

1. Tee **paremklikk andmeallika peal** (vasakus sidebaris) →  **New | Schema**.
2. Pane nimeks midagi arusaadavat, näiteks **`sqltoo`** (sobib ka `harjutus` või
   `companies_db`). Nimi ise ei ole oluline, aga jäta meelde - seda läheb vaja.
3. **OK**.

Skeem ilmub sidebari. Kui ei ilmu, kliki andmeallika nime kõrval olevat **`N of M`**
linki ja tee oma skeemil linnuke.

---

## 4. Loe andmed sisse

Nüüd paneme `companies` tabeli ja 1000 rida sinna skeemi:

1. Tee **paremklikk oma skeemi peal** → **SQL Scripts | Run SQL Script…**.
2. Vali fail **`companies.sql`** ja vajuta **Open**.
3. Oota - 1000 INSERT-i võtab hetke.

Kontrolli, et õnnestus: ava sidebaris skeem → **tables** → seal peab olema `companies`.
Topeltklikk avab andmed, ridu peab olema **1000**.

> Jälgi, et teeksid paremkliki **oma skeemi**, mitte mõne muu peal - muidu tekib tabel
> valesse kohta ja hiljem päringud ei leia seda üles.

---

## 5. Ava SQL konsool ja lahenda ülesanded

1. Tee **paremklikk oma skeemi peal** → **New | Query Console**
   (kiirklahv: `Ctrl+Shift+Q`).
2. Kopeeri `exercise.sql` failist kommentaarid konsooli.
3. Kirjuta iga kommentaari alla oma SQL lause.

**Lause käivitamiseks** pane kursor lause peale ja vajuta **`Ctrl+Enter`** või klõpsa
üleval tööriistaribal **play nuppu**.

> Kui konsoolis on mitu lauset, küsib DataGrip, mida käivitada. Vali **ainult see üks
> lause**, mitte "kõik laused". Muidu jooksevad kõik lause korraga läbi - ja kui seal on
> juba `DELETE` või `UPDATE`, muudad andmeid ära ja järgmised päringud annavad vale
> tulemuse.

Kontrolli iga lause tulemust kohe: tuleb ridu nii palju, kui ootasid? Enamik vigu
(`=<` asemel `<=`, jutumärgid puudu, vale veerunimi) tulevad kohe välja, kui lause
päriselt käivitada.

> **Ülesanded 20-22 muudavad andmeid** (INSERT, UPDATE, DELETE). See on normaalne.
> Kui tahad algse seisu tagasi, kustuta skeem ära ja tee 3.-4. samm uuesti.

---

## 6. Esita töö

1. Kopeeri konsoolist **kommentaarid koos oma lahendustega** tagasi faili
   `exercise.sql`.
2. Salvesta fail.
3. Lae see Teamsis ülesande juurde üles ja vajuta **Turn in**.

Jäta kommentaarid alles - nende järgi on näha, milline lause millise ülesande vastus on.


---

<!-- English version below / Ingliskeelne versioon allpool -->

# SQL exercise - connecting the database to DataGrip

This guide covers every step, from starting Docker to submitting the work in Teams.
If something does not work, read the error message - it almost always says what is wrong.

---

## 1. Start Docker

Clone your class repository and start it:

```bash
git clone https://github.com/Kasparsu/ta24blearndb2
cd ta24blearndb2
docker compose up -d
```

Check that the containers are running:

```bash
docker compose ps
```

The `db` service must be `running`. If it is not, DataGrip cannot connect - everything
below assumes it is up.

The connection details come from `docker-compose.yml`:

| Field | Value |
|---|---|
| Host | `localhost` |
| Port | `33061` |
| User | `root` |
| Password | `example` |
| Database | **leave empty** |

> The port is **33061**, not 3306. `docker-compose.yml` has the line `- 33061:3306`:
> the left side is the port on your machine, the right side the one inside the container.
> DataGrip connects to the one on your machine.
>
> Leave the Database field empty, because `docker-compose.yml` does not create any
> database - you create it yourself in step 3.

---

## 2. Create the connection in DataGrip (first time only)

1. **File | New | Data Source** and pick **MariaDB**.
2. Fill in Host, Port, User and Password from the table above.
3. **Download the drivers.** At the bottom of the window there is a
   **Download missing driver files** link - click it. Without the driver there is no
   connection and DataGrip will show an error. You only need to do this once.
4. **Test Connection** (also at the bottom). Wait for the green confirmation with the
   server version.
5. Only then press **OK**.

> **Always run Test Connection before OK.** If you press OK with a broken connection, you
> will only see the errors later while running queries and assume the problem is in your SQL.

If Test Connection fails:
- is the `db` container running according to `docker compose ps`?
- is the port definitely `33061`?
- is the password `example`?

---

## 3. Create a schema

A new connection has no database yet. Create one:

1. **Right-click the data source** (in the left sidebar) → **New | Schema**.
2. Give it a clear name, for example **`sqltoo`** (`practice` or `companies_db` work too).
   The name itself does not matter, but remember it - you will need it.
3. **OK**.

The schema appears in the sidebar. If it does not, click the **`N of M`** link next to the
data source name and tick your schema.

---

## 4. Load the data

Now put the `companies` table and its 1000 rows into that schema:

1. **Right-click your schema** → **SQL Scripts | Run SQL Script…**.
2. Pick the file **`companies.sql`** and press **Open**.
3. Wait - 1000 INSERTs take a moment.

Check that it worked: open the schema in the sidebar → **tables** → `companies` must be
there. Double-click it to see the data, there must be **1000** rows.

> Make sure you right-click **your own schema** and not some other node, otherwise the
> table ends up in the wrong place and your queries will not find it later.

---

## 5. Open an SQL console and solve the tasks

1. **Right-click your schema** → **New | Query Console**
   (shortcut: `Ctrl+Shift+Q`).
2. Copy the comments from `exercise.sql` into the console.
3. Write your SQL statement under each comment.

**To run a statement**, put the caret on it and press **`Ctrl+Enter`**, or click the
**play button** in the toolbar above.

> When the console holds several statements, DataGrip asks what to run. Pick **that one
> statement only**, not "all statements". Otherwise everything runs at once - and once
> there is a `DELETE` or an `UPDATE` in there, you change the data and every later query
> returns the wrong result.

Check the result of each statement straight away: are there as many rows as you expected?
Most mistakes (`=<` instead of `<=`, missing quotes, a wrong column name) show up
immediately once you actually run the statement.

> **Tasks 20-22 change the data** (INSERT, UPDATE, DELETE). That is normal.
> To get back to the original state, drop the schema and repeat steps 3 and 4.

---

## 6. Submit

1. Copy **the comments together with your solutions** from the console back into
   `exercise.sql`.
2. Save the file.
3. Upload it to the assignment in Teams and press **Turn in**.

Keep the comments - they show which statement answers which task.
