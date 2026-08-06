```mermaid
%% Improved RAMSRAN Security Platform architecture (neat, clear, attractive)
graph LR
  %% Layout hint
  classDef sourceStyle fill:#2c3e50,stroke:#18bc9c,stroke-width:2px,color:#ffffff;
  classDef ingestStyle fill:#2d4263,stroke:#c84b31,stroke-width:2px,color:#ffffff;
  classDef dbStyle fill:#1a1a24,stroke:#a370f7,stroke-width:3px,color:#ffffff;
  classDef engineStyle fill:#0b3c5d,stroke:#328cc1,stroke-width:2px,color:#ffffff;
  classDef alertStyle fill:#3d1e22,stroke:#d9534f,stroke-width:2px,color:#ffffff;
  classDef uiStyle fill:#1d2731,stroke:#00a8cc,stroke-width:2px,color:#ffffff;
  classDef futureStyle stroke-dasharray: 6 4, opacity:0.9, fill:#263242;
  classDef noteStyle fill:#ffffff,stroke:#cbd5e1,stroke-width:1px,color:#0b2436;

  %% --- Data Sources (left) ---
  subgraph Sources ["🔌 Data Sources"]
    direction TB
    Src_Lin[("🐧 Linux Agent")]
    Src_Win[("🪟 Windows Agent")]
    Src_Mac[("🍏 macOS Agent")]
    Src_Cloud(("☁️ Cloud Connectors\n(Future)"))
  end
  class Src_Lin,Src_Win,Src_Mac,Src_Cloud sourceStyle;
  class Src_Cloud futureStyle;

  %% --- Ingestion & Processing ---
  subgraph Ingest ["📥 Ingestion & Processing"]
    direction TB
    Recv(("🌐 Receiver API"))
    Auth(("🔑 Auth & RBAC"))
    Norm(("⚙️ Parser & Normalization"))
  end
  class Recv,Auth,Norm ingestStyle;

  %% --- Central Data Store (center) ---
  subgraph DataStore ["💾 Security Data Store"]
    direction TB
    DB_Log(("📝 Logs DB"))
    DB_Alert(("⚠️ Alerts DB"))
    DB_Rule(("📜 Rules DB"))
    DB_User(("👤 Users DB"))
    DB_Agent(("🤖 Agents DB"))
  end
  class DB_Log,DB_Alert,DB_Rule,DB_User,DB_Agent dbStyle;

  %% --- Analytics Engine (right-center) ---
  subgraph Engine ["🧠 Security Analytics Engine"]
    direction TB
    Detect(("🛡️ Detection Engine\n(Rule Matching)"))
    Correl(("🔄 Correlation Engine\n(Multi-event)"))
    AI(("🤖 AI Analytics\n(Future)"))
  end
  class Detect,Correl engineStyle;
  class AI futureStyle;

  %% --- Alerting & Response (right) ---
  subgraph Alerts ["🚨 Alert & Response Services"]
    direction TB
    Gen(("💥 Alert Generation"))
    Notif(("🔔 Notification Service\n(Slack/Email)"))
  end
  class Gen,Notif alertStyle;

  %% --- UI Layer (top-right) ---
  subgraph UI ["🖥️ RAMSRAN UI"]
    direction TB
    Dash(("📊 Dashboard & Visualization"))
    Invest(("🔍 Investigation & Search"))
    Rep(("📋 Reports & Rules Mgmt"))
  end
  class Dash,Invest,Rep uiStyle;

  %% --- Flows: sources -> ingest -> datastore -> engine -> alerts -> ui ---
  Src_Lin & Src_Win & Src_Mac -->|secure transport| Recv
  Src_Cloud -.->|future connectors| Recv

  Recv -->|api auth| Auth -->|validated| Norm
  Norm -->|normalized events| DB_Log

  DB_Log ==> Detect
  Detect <--> Correl
  Correl <--> AI

  Detect -->|alerts| Gen --> Notif
  Gen --> DB_Alert

  %% UI interactions (queries / rule edits)
  Dash -->|query / visualize| DB_Log
  Dash -->|show alerts| DB_Alert
  Invest -->|search / export| DB_Log
  Rep -->|manage rules| DB_Rule

  %% Metadata / user stores
  Rep --> DB_User
  DB_Agent --> DB_Log

  %% Legend / notes
  subgraph Note [ ]
    style Note fill:none,stroke:none
    NOTE[[<b>Legend</b><br/>Solid arrows = primary data/control flow<br/>Dashed arrows = future/optional<br/>Double arrow (==>) = high-throughput/channel link]]
  end
  class NOTE noteStyle;

  %% Positioning helpers (non-breaking, keeps center alignment)
  NOTE -.-> UI
```
