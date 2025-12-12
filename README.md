# Shardium

A **production-ready, proxy‑less Minecraft Cloud System** designed for modern scaling, automatic orchestration, and seamless player transfers — all without BungeeCord.

---

## 🚀 Überblick

Shardium besteht aus drei Kernkomponenten:

* **Controller** – Statische Crystal Binary (`shardium`) mit CLI & REST‑API
* **Agent Plugin** – Paper‑Plugin auf jedem Server (automatisch in Templates)
* **Kommunikation** – TCP / WebSocket + eigenes Transfer‑Packet‑System

**Ziel:** Eine komplett geschlossene Cloud ohne Proxy, mit Auto‑Scaling, Gruppen‑Management, Live‑Monitoring und nahtlosen Transfers.

---

## 📂 Verzeichnisstruktur

```
[DATA-PFAD]/
├── config/
│   ├── config.yml
│   └── groups/
│       ├── lobby.yml
│       └── game.yml
├── templates/
│   ├── lobby/
│   └── game/
├── jars/
├── static/
├── temp/
└── logs/
```

---

## ⚙️ config.yml (automatisch generiert)

```yaml
dataPath: "/opt/shardium/data"
rest:
  bindIp: "192.168.1.100"
  port: 8081
  public: true
  token: "shardium_xxxxxx"
servers:
  bindIps: ["0.0.0.0", "127.0.0.1"]
  portRange:
    start: 25565
    end: 25999
```

---

## 📄 Gruppen‑Definition (Example: lobby.yml)

```yaml
name: "lobby"
static: true
memory: 1024
maxPlayers: 100
minOnline: 1
maxOnline: 3
software: "paper"
version: "1.21.1"
template: "lobby"
```

---

## 🛠 CLI‑Befehle

### Setup

```
shardium setup
shardium start
shardium stop
shardium status
```

### Gruppen

```
shardium group create
shardium group edit <name>
shardium group list
shardium group info <name>
```

### Services

```
shardium service create <group>
shardium service list
shardium service start <id>
shardium service stop <id>
shardium service logs <id>
shardium service console <id>
shardium service transfer <player> <target>
```

### Monitoring

```
shardium logs all
shardium logs <group>
shardium metrics
shardium network list
shardium cache clear
```

---

## 🔌 Agent Kommunikation

### Agent → Controller

```json
HELLO {...}
HEARTBEAT {...}
PLAYER_JOIN {...}
PLAYER_QUIT {...}
```

### Controller → Agent

```json
COMMAND { type: "shutdown" }
COMMAND { type: "console" }
COMMAND { type: "transfer" }
COMMAND { type: "drain" }
```

---

## 🌐 REST‑API

```
GET  /services
GET  /services/<id>
POST /services/<group>
POST /transfer
GET  /metrics
GET  /groups
Authorization: Bearer <token>
```

---

## 🧩 Agent‑API (Paper / ServicesManager)

```java
NetworkApi api = Bukkit.getServicesManager().getRegistration(NetworkApi.class).getProvider();

NetworkService local = api.getLocalService();
List<NetworkService> services = api.listServices("lobby");
api.transferPlayer(player, "lobby-XYZ123");
api.sendNetworkMessage("global.chat", data);
```

---

## 🔄 Beispiel‑Workflow

```
1. ./shardium setup
2. ./shardium group create
3. ./shardium service create lobby
4. ./shardium start
5. Spieler joinen → Monitoring aktiv
6. Live Logs ansehen
7. Spieler transferieren
```

---

## 📦 Build & Deployment

```
crystal build src/Shardium.cr --release -static -o shardium
sudo cp shardium /usr/local/bin/
sudo ./shardium setup --data /opt/shardium/data
sudo systemctl start shardium
```

---

## 🎯 Feature Matrix

| Feature                       | Status |
| ----------------------------- | ------ |
| Proxy‑frei (eigene Transfers) | ✅      |
| Interaktive Wizards           | ✅      |
| Live‑Logs                     | ✅      |
| Console‑Attach                | ✅      |
| Auto‑Scaling                  | ✅      |
| Registry/Cache System         | ✅      |
| REST‑API (Bearer Auth)        | ✅      |
| Static Binary                 | ✅      |
| Agent‑API                     | ✅      |

---

**Shardium – Die moderne Cloud für Minecraft‑Server.**
