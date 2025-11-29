# 🎨 Tapestry — Cross-Project Knowledge Federation

**Weaves individual project knowledge into organization-wide patterns and shared learning.**

Part of the [Claude Textile](https://github.com/hgeldenhuys/claude-textile) ecosystem.

---

## 🎯 Vision

Each project captures its own institutional knowledge via **Weave**. But organizational knowledge lives *across* projects:

- **Patterns** used in multiple projects
- **Solutions** discovered in one project, applicable to others
- **Pain points** that recur across teams
- **Architectural decisions** that need consistency
- **Best practices** that should be standardized

**Tapestry** aggregates knowledge from individual Weave instances and makes it queryable across the organization.

---

## 🏗️ Architecture

### Core Concepts

#### Knowledge Federation
Individual projects maintain their own Weave instances. Tapestry aggregates and synthesizes knowledge without centralizing data ownership.

```
Project A (Weave)          Project B (Weave)        Project C (Weave)
    ↓                           ↓                          ↓
    │                           │                          │
    └───────────────┬───────────┴──────────────┬───────────┘
                    ↓                          ↓
           Tapestry (Federation)
                    ↓
       Cross-project patterns
       Shared knowledge catalog
       Best practices registry
```

#### Knowledge Dimensions in Tapestry

Each dimension aggregates across projects:

- **Ontology (O)** - Shared entities, unified data models
- **Qualia (Q)** - Recurring pain points, proven solutions
- **Epistemology (E)** - Validated patterns, high-confidence knowledge
- **Praxeology (Π)** - Ways of working across teams
- **Axiology (A)** - Organizational values, trade-offs, standards
- **Teleology (T)** - Organizational goals, architecture principles
- **History (Η)** - Evolution of decisions, timeline of changes
- **Modality (Μ)** - Alternative approaches considered, lessons learned

### Storage Model

**Distributed Storage**
- Projects keep their Weave knowledge locally
- Tapestry maintains aggregate indexes and catalogs
- Sync happens via lightweight change feeds (not full replication)

```
.tapestry/
├── config.json              # Federation config
├── catalog/
│   ├── entities.json        # Unified ontology
│   ├── patterns.json        # Cross-project patterns
│   ├── solutions.json       # Proven solutions
│   └── guidelines.json      # Best practices
├── synced-projects/
│   ├── project-a/
│   │   ├── metadata.json
│   │   └── last-sync.json
│   ├── project-b/
│   └── project-c/
├── analytics/
│   ├── pain-points.json     # Recurring issues
│   ├── solutions-map.json   # Solution to pain-point mappings
│   └── adoption-metrics.json # Pattern adoption rates
└── federation/
    ├── peers.json           # Connected Tapestry instances (teams, orgs)
    └── change-log.jsonl     # Federated change events
```

---

## 🚀 Usage

### For Projects (Opt-in Sharing)

Projects decide what knowledge to share with Tapestry:

```bash
# Install Tapestry support into a project
tapestry install /path/to/project

# Configure what to share
# Edit .tapestry-sync/config.json
```

**Config Example:**
```json
{
  "enabled": true,
  "projectName": "agios-crm",
  "projectDomain": "crm",
  "syncInterval": 3600000,
  "sharePolicy": {
    "ontology": ["entities", "relations"],
    "qualia": ["painPoints", "solutions"],
    "praxeology": ["wowPatterns", "bestPractices"],
    "epistemology": ["patterns"],
    "exclude": ["secretSauces", "inProgress"]
  },
  "federation": {
    "enabled": false,
    "centralTapestry": "tapestry.internal.company.com"
  }
}
```

### For Organizations (Knowledge Hub)

Set up Tapestry to aggregate knowledge from multiple projects:

```bash
# Initialize Tapestry
tapestry init --mode federation

# Register projects
tapestry register-project agios-crm https://github.com/company/agios-crm
tapestry register-project weave-platform https://github.com/company/weave-platform
tapestry register-project docs-site https://github.com/company/docs-site

# Start syncing
tapestry sync --continuous
```

### Querying Cross-Project Knowledge

```typescript
import { Tapestry } from 'claude-tapestry';

const tapestry = new Tapestry({
  configPath: '.tapestry/config.json'
});

// Find how other projects solved a problem
const solutions = await tapestry.findSolutions('authentication', {
  minConfidence: 0.7,
  projects: ['all'] // or specific projects
});

// Get patterns used across projects
const patterns = await tapestry.getPatterns('domain:crm', {
  adoptionThreshold: 0.5 // used in at least 50% of CRM projects
});

// Check if an approach is against established guidelines
const conflicts = await tapestry.checkAgainstGuidelines({
  approach: 'using-custom-orm',
  domain: 'backend'
});
```

### For Claude Code Sessions

Query Tapestry knowledge in Claude Code:

```bash
# Create Tapestry agent for the session
/tapestry:create

# Query organizational patterns
/tapestry:patterns crm how-to-handle-permissions

# Find proven solutions
/tapestry:solutions how-have-other-teams-done-multi-tenant

# Check guidelines
/tapestry:guidelines what-are-our-standards-for-api-design
```

---

## 📊 Analytics

### Pain Point Analysis

```bash
# What problems recur across projects?
bun tapestry/analytics.ts pain-points --by-domain

# Output:
# Backend: authentication (5 projects), rate-limiting (4 projects)
# Frontend: state-management (6 projects), performance (5 projects)
# DevOps: secret-rotation (3 projects), monitoring (4 projects)
```

### Solution Effectiveness

```bash
# How effective are our solutions?
bun tapestry/analytics.ts solution-metrics

# Output:
# Solution: dependency-injection
#   Used in: 7 projects
#   Adoption rate: 85%
#   Success rate: 92%
#   Average implementation time: 3 days
```

### Pattern Adoption

```bash
# Which patterns are teams adopting?
bun tapestry/analytics.ts adoption-trends

# Output:
# CQRS Pattern: 30% → 65% (Jan-Dec 2024)
# Event Sourcing: 15% → 45%
# GraphQL: 10% → 55%
```

---

## 🔌 Integration Points

### With Weave

Each Weave instance periodically syncs with Tapestry:

```typescript
// In project's Weave instance
const weave = new Weave({ projectDir: '.agent/weave' });
const tapestry = new Tapestry();

// Sync high-confidence knowledge to Tapestry
await weave.syncToTapestry(tapestry, {
  minConfidence: 0.7,
  dimensions: ['qualia', 'praxeology', 'epistemology']
});
```

### With Spindle

Spindle manages Tapestry agent sessions:

```typescript
const spindle = new Spindle();
const tapestryAgent = await spindle.getOrCreateSession('tapestry', {
  knowledge: tapestryKnowledge,
  contextSize: 20000 // Larger context for cross-project data
});
```

### With Loom

Loom workflows can query Tapestry patterns:

```typescript
// In a Loom workflow
const patterns = await tapestry.getPatterns('domain:backend');
const story = await loom.createStory({
  title: 'Implement authentication',
  similarPatterns: patterns,
  provenSolutions: await tapestry.findSolutions('authentication')
});
```

---

## 🌍 Federation

### Team/Organization Federation

Multiple teams can operate their own Tapestry instances and federate knowledge:

```json
{
  "federation": {
    "enabled": true,
    "mode": "P2P",
    "peers": [
      {
        "name": "backend-team",
        "url": "tapestry.backend.internal",
        "sync": true
      },
      {
        "name": "frontend-team",
        "url": "tapestry.frontend.internal",
        "sync": true
      }
    ],
    "policy": {
      "trustLevel": "high",
      "conflictResolution": "merge",
      "deduplication": true
    }
  }
}
```

### Multi-Organization Federation

In the future, organizations can choose to federate with industry peers:

```json
{
  "federation": {
    "mode": "FEDERATED",
    "externalPeers": [
      {
        "name": "industry-ai-knowledge-base",
        "url": "knowledge.aidevs.org",
        "readOnly": true,
        "trustLevel": "medium"
      }
    ]
  }
}
```

---

## 📈 Metrics & Monitoring

### Knowledge Health

```bash
# Check knowledge base health
bun tapestry/health.ts status

# Output:
# Total aggregated knowledge: 12,450 facts
# Entities: 342
# Patterns: 87 (confidence: 0.78 avg)
# Solutions: 156 (success rate: 89%)
# Projects synced: 8 (last 24h: 8)
# Duplicate facts detected: 23
```

### Sync Status

```bash
# Monitor Tapestry sync health
bun tapestry/health.ts sync-status

# Output:
# Project A: Last sync 2h ago ✓
# Project B: Last sync 18h ago ⚠️
# Project C: Last sync 3d ago ✗
# Project D: Syncing now... (45%)
```

---

## 🔧 Implementation Roadmap

### Phase 1 (MVP) — In Development
- [ ] Local Tapestry instance setup
- [ ] Project registration and sync
- [ ] Basic analytics (pain points, solutions)
- [ ] Claude Code integration (read-only queries)
- [ ] Pain point aggregation

### Phase 2 — Planned
- [ ] Spindle agent for Tapestry queries
- [ ] Federation (team-level P2P)
- [ ] Conflict resolution
- [ ] Advanced analytics dashboard
- [ ] Best practices catalog
- [ ] Automated guideline enforcement

### Phase 3 — Future
- [ ] Multi-org federation
- [ ] Privacy-preserving knowledge exchange
- [ ] Cross-org pattern discovery
- [ ] Industry knowledge partnerships
- [ ] ML-powered insight extraction

---

## 🤝 Integration with Claude Textile

```
Individual Projects
    ↓
Weave (captures knowledge)
    ↓
Tapestry (aggregates across projects)
    ↓
Organization (discovers patterns)
    ↓
Teams can query & learn from each other
```

### Data Flow

```
Project A writes fact → Weave
                          ↓
                   Tapestry syncs (every 1h)
                          ↓
Project B queries pattern → Tapestry
                             ↓
                   Returns patterns + solutions
                             ↓
Project B implements (faster, higher confidence)
```

---

## 📚 Documentation

- **[Installation Guide](./INSTALL.md)** - Single project vs organization setup
- **[Configuration](./CONFIG.md)** - Sync policies, federation, sharing rules
- **[API Reference](./API.md)** - Querying, analytics, federation APIs
- **[Analytics](./ANALYTICS.md)** - Pain point analysis, metrics, reporting
- **[Federation Guide](./FEDERATION.md)** - Team and cross-org federation
- **[Troubleshooting](./TROUBLESHOOTING.md)** - Sync issues, data conflicts

---

## 🚀 Getting Started

### Single Organization

```bash
# 1. Install Tapestry
git clone --depth 1 git@github.com:hgeldenhuys/claude-tapestry.git /tmp/tapestry && /tmp/tapestry/install.sh && rm -rf /tmp/tapestry

# 2. Register projects
cd /path/to/org
tapestry register-project my-api ./projects/api
tapestry register-project my-frontend ./projects/frontend

# 3. Start syncing
tapestry sync --continuous

# 4. Query in Claude Code
/tapestry:patterns backend how-to-implement-caching
```

### Team Federation

```bash
# Team A sets up Tapestry
tapestry init --mode federation
tapestry sync --server --port 8080

# Team B connects to Team A's Tapestry
tapestry federate --peer tapestry.team-a.internal
tapestry sync --continuous
```

---

## 📄 License

MIT License

---

## 🙏 Philosophy

Tapestry embodies the principle that **organizations get smarter when they systematically learn from each other**.

Like a tapestry woven from many threads, Tapestry:
- Brings together knowledge from many projects
- Reveals patterns invisible at individual project level
- Amplifies good ideas (they get reused faster)
- Prevents repeated mistakes (solutions are discoverable)
- Accelerates learning (new projects benefit from prior art)

---

**Questions?** Open an issue in this repository.

**Built as part of [Claude Textile](https://github.com/hgeldenhuys/claude-textile).**
