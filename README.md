# OnAirHQ 

A self-hosted environment for understanding, running, and exploring the OnAirHQ ecosystem.

# Architecture

The following diagram illustrates a typical integration between these components. The exact control paths depend on the OnAirHQ implementation.


```mermaid
flowchart TB
    User["👤 Ascultător"]
    Web["🌐 Web Player<br/>(Next.js)"]
    IC["📡 Icecast<br/>(server de streaming)"]
    LS["🎛️ Liquidsoap<br/>(automatizare radio)"]
    GW["🔌 s3sg<br/>(gateway intern S3)"]
    S3["☁️ Storage S3<br/>(melodii + jingle-uri)"]
    Disk["💾 Foldere locale<br/>(tracks / jingles)"]
    DG["🎵 Discogs API<br/>(coperți album)"]

    User -->|"1. Deschide pagina"| Web
    Web -->|"2. Cere streamul audio<br/>GET /radio.mp3"| IC
    IC -->|"3. Trimite audio MP3 continuu<br/>+ titlu melodie + cover URL"| Web
    Web -.->|"4. Cere starea / melodia curentă<br/>(dedus, de verificat)"| IC

    LS -->|"A. Trimite audio MP3 + parolă<br/>(conexiune de tip 'source')"| IC
    LS -->|"B. Trimite titlu + cover<br/>GET /admin/metadata"| IC
    LS -->|"C. Cere melodie / jingle<br/>GET /stream/tracks sau /jingles"| GW
    GW -->|"D. Citește fișierele audio"| S3
    LS -.->|"C'. Alternativ: citește fișiere"| Disk
    LS -->|"E. Caută coperta<br/>GET /database/search"| DG
```
