# StreamIQ - MongoDB Database Project
# StreamIQ — Music Streaming Document Database
### CS3200 Project 2: Design & Implement a Document Database

---

## Overview

StreamIQ is a music streaming platform database originally designed as a normalized relational database in SQLite3 (Project 1) and adapted here to a document-based MongoDB database (Project 2). The platform models core entities including users, artists, albums, songs, playlists, listen history, and streaming analytics.

---

## Database Design

The 10 normalized relational tables from Project 1 were restructured into **3 root collections** using embedded documents and denormalization following MongoDB best practices:

| Collection | What's Embedded |
|---|---|
| `users` | Playlists (with songs), followed artists, listening snapshots |
| `artists` | Albums (with songs, each song has credited artist roles) |
| `listenHistory` | Denormalized song snapshot (title, artist, album) for fast reads |

> `listenHistory` is kept as a separate root collection rather than embedded in users because play events grow unboundedly — embedding millions of records inside a user document would exceed MongoDB's 16MB document limit.

---

## Repository Contents

```
├── data/
│   ├── users.json                  # 10 user documents with embedded playlists
│   ├── artists (1).json            # 5 artist documents with embedded albums/songs
│   └── listenHistory (1).json      # 79 listen event documents
├── queries/
│   ├── query1_aggregation.js       # Top 5 most-played songs (aggregation framework)
│   ├── query2_complex_search.js    # Explicit/long songs with $or logical connectors
│   ├── query3_count_documents.js   # Per-user listen count + monthly breakdown
│   ├── query4_update_document.js   # Toggle isExplicit boolean flag on songs
│   ├── query5_user_dashboard.js    # User dashboard summary
│   └── queries.js                  # All five queries in one file
├── app/
│   └── streamiq-app/               # Node + Express CRUD application (Part 6)
├── DB_pj1_requireddoc.pdf          # Problem requirements document
├── uml-diagram.pdf                 # UML Class Diagram
├── erd-mongo.pdf                   # MongoDB ERD (hierarchical logical model)
├── IMPORT_INSTRUCTIONS.md          # How to initialize the database
└── README.md
```

---

## Diagrams

- **UML Class Diagram:** https://lucid.app/lucidchart/c9bd40e4-83f2-448a-9769-c81f1e9cafdc/edit?invitationId=inv_4d8b1b3d-a69b-47f9-8abd-9f3148d1c77b&page=0_0#
- **MongoDB ERD (Hierarchical Logical Model):** https://lucid.app/lucidchart/bcc77c0b-a90d-45d7-b746-c14ecd1f02a4/edit?invitationId=inv_ef9d8c28-5e08-403c-91f9-f3bee7038371&page=0_0#

---

## Database Setup

### Prerequisites
- MongoDB v6.0 or later
- Node.js v18 or later
- mongoimport CLI (included with MongoDB Database Tools)

### Import Data

```bash
mongoimport --db streamiq --collection users        --file data/users.json
mongoimport --db streamiq --collection artists      --file "data/artists (1).json"
mongoimport --db streamiq --collection listenHistory --file "data/listenHistory (1).json"
```

Expected document counts after import:
- `users` — **10 documents**
- `artists` — **5 documents**
- `listenHistory` — **79 documents**

See `IMPORT_INSTRUCTIONS.md` for full details including MongoDB Compass instructions and how to reset the database.

---

## Queries

Five queries are defined in the `queries/` folder, each satisfying a specific assignment requirement:

| Query | File | Requirement |
|---|---|---|
| 1 | `query1_aggregation.js` | Aggregation framework — top 5 most-played songs |
| 2 | `query2_complex_search.js` | Complex search with `$or` logical connectors |
| 3 | `query3_count_documents.js` | Count documents for a specific user |
| 4 | `query4_update_document.js` | Update — toggle `isExplicit` boolean flag |
| 5 | `query5_user_dashboard.js` | User dashboard summary across all users |

### Running the queries

```bash
mongosh streamiq
load("queries/queries.js")
```

Or run them individually:
```bash
mongosh streamiq --file queries/query1_aggregation.js
```

---

## Node + Express Application (Part 6)

A full-stack CRUD web application is included in the `streamiq-app/` folder. It supports create, display, modify, and delete operations on all three collections via a browser-based interface.

### Run the app

```bash
cd streamiq-app
npm install
npm start
```

Then open **http://localhost:3000** in your browser.

### Features

- **Users** — Create, view, edit, delete users; toggle subscription tier (Free ↔ Premium)
- **Artists** — Create, view, edit, delete artists; expand full album/song catalog; toggle `isExplicit` flags per album
- **Listen History** — Browse play events by user; log new plays; view per-user play counts; delete events

See `streamiq-app/README.md` for the full API reference.

---

## AI Usage

AI tools (Claude by Anthropic) were used to assist with generating mock data, structuring JSON collection examples, drafting MongoDB queries, and building the Node + Express application.

---

## Video Demonstration

*[Link to be added after recording]*
