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
