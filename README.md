# API dokumentasjon for botanisk forenings aktivitetskalender

**GET** `/wp-json/calendar/v1/events/json`

---

## Query-parametre

- `fromDate` _(valgfri)_  
  UNIX-timestamp i **millisekunder**. Returnerer kun events med `to` ≥ `fromDate`.  
  Eksempel: `?fromDate=1704067200000` (1. jan 2024 00:00).

- `toDate` _(valgfri)_  
  UNIX-timestamp i **millisekunder**. Returnerer kun events med `to` ≤ `toDate`.  
  Eksempel: `?toDate=1706745599000` (31. jan 2024 23:59).

> Dersom ingen datoer settes, hentes alle events fra “nå” og 4 år fram i tid.

- `category` _(valgfri)_  
  Filtrer på kategori‐ID.

- `organizer` _(valgfri)_  
  Filtrer på arrangør‐ID.

- `page` _(valgfri)_  
  Sidenummer (heltall ≥ 1). Default: `1`.

---

## Paginering

Responsobjektet inneholder:

| Felt    | Type    | Beskrivelse                               |
| ------- | ------- | ----------------------------------------- |
| `items` | `array` | Liste av event‐objekter (maks 25 per side)|
| `count` | `int`   | Totalt antall events som matcher filteret |
| `max`   | `int`   | Side‐størrelse (alltid 25)                |
| `page`  | `int`   | Gjeldende side                            |
| `start` | `int`   | Indeks på første element (1-basert)       |
| `end`   | `int`   | Indeks på siste element (1-basert)        |

For neste side:  
```http
GET /wp-json/calendar/v1/events/json?...&page=2
```

## Eksempel på JSON-respons fra `/wp-json/calendar/v1/events/json`

Nedenfor ser du et kort eksempel på hvordan responsen fra API-endepunktet kan se ut når du henter én enkelt event. Feltene `count`, `max`, `page`, `start` og `end` viser paginering — her er det totalt 128 events, 25 per side, og denne kallet viser side 1 (element 1–25).

```json
{
  "items": [
    {
      "title": "Kartleggingsprogram, 2025",
      "content": "Kartleggingsgruppen er åpen for alle. Her er årets program. Kontakt Alf Harry Øygarden for mer informasjon.",
      "id": 14288,
      "from": "1741765200",
      "to": "1751234400",
      "isAllDay": "",
      "isCancelled": "",
      "location": "",
      "organizer": {
        "id": "15",
        "name": "Sunnhordland Botaniske Forening",
        "selected": ""
      },
      "image": "https://botaniskforening.no/wp-content/uploads/2021/04/Tiriltunge-2000x500.jpg",
      "url": "https://botaniskforening.no/kalender/kartleggingsprogram-2025",
      "categories": ["Foredrag", "Kartlegging", "Kurs", "Møte", "Tur"],
      "isFull": false,
      "county": "Hordaland",
      "municipality": ""
    }
  ],
  "count": 128,
  "max": 25,
  "page": 1,
  "start": 1,
  "end": 25
}
```

## Struktur for hvert `item` i `items`-arrayen

Hvert element i `items`-arrayen representerer én aktivitet (event) og har følgende felter:

| Felt          | Type        | Beskrivelse                                                                                 |
| ------------- | ----------- | ------------------------------------------------------------------------------------------- |
| `title`       | `string`    | Tittel på aktiviteten.                                                                     |
| `content`     | `string`    | Beskrivelse eller innholdstekst (forkortet).                                               |
| `id`          | `int`       | Unik ID for eventet (post ID i WordPress).                                                 |
| `from`        | `string`    | Starttidspunkt som UNIX-timestamp (i **sekunder**).                                        |
| `to`          | `string`    | Sluttidspunkt som UNIX-timestamp (i **sekunder**).                                         |
| `isAllDay`    | `string`    | `"1"` hvis heldagsaktivitet, tom streng ellers.                                            |
| `isCancelled` | `string`    | `"1"` hvis avlyst, tom streng ellers.                                                      |
| `location`    | `string`    | Adresse eller koordinater (lat,long) som streng.                                           |
| `organizer`   | `object`    | Arrangør‐objekt med feltene:                                                                   |
|               |             | - `id` (`string`): Arrangørens ID.                                                         |
|               |             | - `name` (`string`): Navn på arrangøren.                                                  |
|               |             | - `selected` (`string`): Intern bruk (vanligvis tom).                                      |
| `image`       | `string`    | URL til eventets forsidebilde.                                                             |
| `url`         | `string`    | Lenke til eventets egen side på nettstedet.                                                |
| `categories`  | `array`     | Liste av kategorinavn (`string`) som eventet tilhører.                                     |
| `isFull`      | `boolean`   | `true` dersom maksimal påmeldingsgrense er nådd, `false` ellers.                           |
| `county`      | `string`    | Fylkesnavn der aktiviteten finner sted (f.eks. `"Hordaland"`).                             |
| `municipality`| `string`    | Kommunenavn (f.eks. `"Bergen"`); tom streng hvis ikke satt.                                |

### Merknader

- `from` og `to` er UNIX-tidsstempler i sekunder. For eksempel representerer `"1741765200"` et bestemt tidspunkt i 2025.  
- `isAllDay` og `isCancelled` er tomme strenger (`""`) når de er `false`.  
- `location` kan være tomt dersom ingen adresse eller koordinater er angitt.  
- `categories` er alltid en liste (kan være tom).  
- Feltene `county` og `municipality` gir mulighet for geografisk filtrering og visning.
