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
