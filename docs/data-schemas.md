# Data Schemas

JSON data files live in `src/data/`. These are imported by Astro components to render content.

---

## shows.json

Each entry represents a live show (past or upcoming). Components filter by date to separate upcoming from past.

```json
[
  {
    "date": "2026-06-15",
    "venue": "Palladium Riga",
    "city": "Rīga",
    "country": "Latvia",
    "ticketUrl": "https://example.com/tickets",
    "description": "Album release show"
  },
  {
    "date": "2026-07-20",
    "venue": "Blues Club Berlin",
    "city": "Berlin",
    "country": "Germany",
    "ticketUrl": null,
    "description": ""
  }
]
```

| Field         | Type   | Required | Notes                                      |
| ------------- | ------ | -------- | ------------------------------------------ |
| `date`        | string | yes      | ISO format `YYYY-MM-DD`                    |
| `venue`       | string | yes      | Venue name                                 |
| `city`        | string | yes      | City name                                  |
| `country`     | string | yes      | Country name                               |
| `ticketUrl`   | string | no       | Link to tickets, `null` if not available   |
| `description` | string | no       | Optional note (e.g., "Album release show") |

---

## members.json

Each entry represents a band member displayed in the About section.

```json
[
  {
    "name": "Jānis Bērziņš",
    "instrument": "Vocals / Harmonica",
    "bio": "Short 1-2 sentence bio about this member.",
    "photo": "/images/members/janis.jpg",
    "socialLinks": [
      { "platform": "facebook", "url": "https://www.facebook.com/username", "label": "Jānis Bērziņš" },
      { "platform": "facebook", "url": "https://www.facebook.com/pagename", "label": "JS Music" }
    ]
  },
  {
    "name": "Mārtiņš Kalniņš",
    "instrument": "Guitar",
    "bio": "Another short bio.",
    "photo": "/images/members/martins.jpg",
    "socialLinks": [
      { "platform": "youtube", "url": "https://www.youtube.com/@channel", "label": "YouTube" }
    ]
  }
]
```

| Field         | Type   | Required | Notes                                |
| ------------- | ------ | -------- | ------------------------------------ |
| `name`        | string | yes      | Full display name                    |
| `instrument`  | string | yes      | Instrument(s) played                 |
| `bio`         | string | yes      | 1-2 sentences                        |
| `photo`       | string | yes      | Path relative to `public/` directory |
| `socialLinks` | array  | no       | Optional list of social profile links; omit or use `[]` if none |

### socialLinks entry

| Field      | Type   | Notes                                              |
| ---------- | ------ | -------------------------------------------------- |
| `platform` | string | `"facebook"` \| `"youtube"` \| `"fiverr"`         |
| `url`      | string | Full URL to the profile or page                    |
| `label`    | string | Display name shown next to the icon in the overlay |

Multiple entries with the same platform are allowed (e.g. three separate Facebook links for different pages).

**Interaction:** social links appear as an overlay on the member photo — fade in on hover (desktop) and tap-to-toggle on mobile. Icons are inline SVG; Fiverr uses its branded green circle mark, Facebook and YouTube use white monochrome marks.

---

## discography.json

Each entry represents an album displayed in the Discography section.

```json
[
  {
    "title": "Midnight Train",
    "year": 2024,
    "cover": "/images/albums/midnight-train.jpg",
    "spotifyUrl": "https://open.spotify.com/album/...",
    "deezerUrl": "https://www.deezer.com/album/...",
    "youtubeUrl": "https://youtube.com/..."
  },
  {
    "title": "Delta Roots",
    "year": 2021,
    "cover": "/images/albums/delta-roots.jpg",
    "spotifyUrl": "https://open.spotify.com/album/...",
    "deezerUrl": null,
    "youtubeUrl": "https://youtube.com/..."
  }
]
```

| Field        | Type   | Required | Notes                                    |
| ------------ | ------ | -------- | ---------------------------------------- |
| `title`      | string | yes      | Album title                              |
| `year`       | number | yes      | Release year                             |
| `cover`      | string | yes      | Path to cover art, relative to `public/` |
| `spotifyUrl` | string | no       | Spotify album link, `null` if N/A        |
| `deezerUrl`  | string | no       | Deezer album link, `null` if N/A         |
| `youtubeUrl` | string | no       | YouTube link, `null` if N/A              |
