# OnAirHQ 

A self-hosted environment for understanding, running, and exploring the OnAirHQ ecosystem.

# Architecture

The following diagram illustrates a typical integration between these components. The exact control paths depend on the OnAirHQ implementation.

```mermaid
flowchart TB
    User["User"]
    Browser["Web Browser"]
    UI["OnAirHQ Web UI"]
    API["Application API / Backend"]
    LS["Liquidsoap"]
    IC["Icecast"]
    Audio["Audio Source"]

    User --> Browser
    Browser --> UI
    UI -->|"HTTP / API requests"| API
    API -.->|"Control commands, if configured"| LS
    Audio -->|"Audio input"| LS
    LS -->|"Encoded audio stream"| IC
    IC -->|"Audio stream"| Browser
    API -.->|"Status / metadata queries"| IC
```
