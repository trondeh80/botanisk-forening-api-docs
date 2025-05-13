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
