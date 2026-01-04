# HNET Registry

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-17-336791.svg)](https://www.postgresql.org/)
[![Go](https://img.shields.io/badge/Go-1.21+-00ADD8.svg)](https://golang.org/)

A web-based management system for the HNET network — a modern hobbyist revival of BITNET/EARN using NJE (Network Job Entry) protocols over TCP/IP.

## 🌐 What is HNET?

HNET is a hobbyist network that revives the historical BITNET (1981-1996) and EARN (1983-1994) academic networks, which connected mainframes worldwide using IBM's Network Job Entry (NJE) protocol. The HNET Registry provides centralized management for modern NJE nodes running on platforms from z/VM to Hercules emulators.

## ✨ Features

- 📋 **Node Registry** — Central database of all registered NJE nodes
- 🤝 **Peering Workflow** — Formal request/approval process for new nodes to join
- 🗺️ **Network Visualization** — Interactive topology and geographic maps
- 🛣️ **Routing Tables** — Automatic generation in multiple formats (RSCS, JES2, NJE38)
- 🔐 **Authentication** — JWT tokens for web UI, API keys for automation
- 👥 **Role-Based Access** — Admin, hub operator, and node operator roles
- 🔌 **REST API** — Full API for third-party integration

## 🏗️ Architecture

### Technology Stack

| Component | Technology |
|-----------|-----------|
| Backend | Go 1.21+ with Chi/Gin |
| Database | PostgreSQL 14+ |
| Frontend | htmx + Go templates |
| CSS | Pico CSS / Simple.css |
| Maps | Leaflet.js (geographic), vis.js (topology) |
| Authentication | JWT + API Keys (bcrypt) |
| ORM | sqlc (type-safe SQL) |
| Migrations | golang-migrate |

### Domain Model

```
Hub Node (e.g., DEARN)
├── Leaf Node (e.g., DRNBRX3A)
│   └── Downstream Node (e.g., DRNBRX3B)
└── Leaf Node (e.g., DFNGATEW)
```

**Networks**: EARN (European) and BITNET (US)
**Nodes**: Hub nodes route traffic; leaf nodes connect to hubs
**Links**: Bidirectional connections between nodes
**Routing**: BFS shortest-path algorithm generates routing tables

## 🚀 Quick Start

### Prerequisites

- Go 1.21 or higher
- PostgreSQL 14+
- git

### Development Setup

```bash
# Clone the repository
git clone git@github.com:mvslovers/hnet-registry.git
cd hnet-registry

# Set environment variables
export DB_PASSWORD="your_password"
export JWT_SECRET="$(openssl rand -base64 32)"

# Install dependencies
go mod download

# Run database migrations
make migrate-up

# Start the server
make run

# Server runs at http://localhost:8080
```

### Using Docker (Development Database)

The repository includes a pre-configured PostgreSQL container:

```bash
cd container/postgresql17
docker build -t hnet-postgres .
docker run -d -p 5432:5432 --name hnet-db hnet-postgres
```

Connection details:
- Host: `localhost` (or `192.168.64.3` for remote)
- Port: `5432`
- User: `postgres`
- Password: `password`
- Database: `hnetdb`

## 📚 Documentation

- [Technical Specification](docs/HNET_Registry_Specification.md) — Complete system design and API reference
- [CLAUDE.md](CLAUDE.md) — Development guidance for AI assistants

## 🗺️ Roadmap

### Phase 1: Core Backend ✅ (In Progress)
- [x] Project setup
- [ ] Database migrations
- [ ] User authentication (JWT)
- [ ] Node CRUD operations
- [ ] Link management
- [ ] Role-based access control

### Phase 2: Peering & Routing
- [ ] Peering request workflow
- [ ] Request approval/rejection
- [ ] BFS routing algorithm
- [ ] Multi-format export (RSCS, JES2, NJE38)

### Phase 3: Frontend & Visualization
- [ ] htmx-based web interface
- [ ] Node management UI
- [ ] Network topology map
- [ ] Geographic map
- [ ] Peering request UI

### Phase 4: Agent & Polish
- [ ] C monitoring agent
- [ ] Status heartbeat integration
- [ ] Docker deployment
- [ ] Documentation
- [ ] Testing

## 🔑 API Overview

### Public Endpoints

```http
GET  /api/v1/nodes              # List all nodes
GET  /api/v1/nodes/{nodename}   # Node details
GET  /api/v1/map                # Network topology
GET  /api/v1/stats              # Network statistics
```

### Authenticated Endpoints

```http
POST /api/v1/auth/register      # Register user
POST /api/v1/auth/login         # Get JWT token
POST /api/v1/peering/requests   # Submit peering request
GET  /api/v1/routing/{node}/rscs # Get routing table
```

See the [API specification](docs/HNET_Registry_Specification.md#6-rest-api-specification) for complete documentation.

## 🤝 Contributing

Contributions are welcome! This project follows a phased implementation approach. Please:

1. Check the current phase in the roadmap
2. Read the [technical specification](docs/HNET_Registry_Specification.md)
3. Follow Go best practices and project structure
4. Write tests for new functionality
5. Update documentation as needed

## 📖 Background

### What is NJE?

Network Job Entry (NJE) is IBM's store-and-forward protocol for mainframe communication, supporting:
- File transfer between mainframes
- Remote job submission
- Interactive messaging (CP MSG)
- Remote command execution

### Historical Networks

- **BITNET** (1981-1996): "Because It's Time Network" — US university network
- **EARN** (1983-1994): European Academic and Research Network
- **HNET** (2010s-present): Hobbyist revival maintaining the retro computing tradition

### NJE Implementations

| Platform | Software | Notes |
|----------|----------|-------|
| z/VM | RSCS | IBM's official implementation |
| z/OS | JES2/JES3 | Native NJE support |
| MVS 3.8 | NJE38 | Bob Polmanter's Hercules implementation |
| Linux/BSD | UnixNJE | Hebrew University, maintained by Moshix |
| OpenVMS | JNET | VAX/VMS implementation |

## 📜 License

This project is licensed under the MIT License - see the [specification](docs/HNET_Registry_Specification.md#0-license) for details.

```
Copyright (c) 2026 HNET Contributors
```

## 🔗 Resources

- [IBM NJE Formats and Protocols](https://www.ibm.com/docs/en/zos/latest?topic=nje-network-job-entry-formats-protocols)
- [UnixNJE on GitHub](https://github.com/moshix/UnixNJE)
- [HNET Website](https://www.moshix.tech)
- [EARN History](https://earn-history.net)
- [BITNET on Wikipedia](https://en.wikipedia.org/wiki/BITNET)

## 🙏 Acknowledgments

This project honors the legacy of BITNET and EARN, which connected researchers and academics worldwide during the early days of academic networking. Special thanks to the HNET community and maintainers of modern NJE implementations.

---

**Status**: 🚧 Early Development — Specification complete, implementation in progress

For questions or support, please open an issue on GitHub.
