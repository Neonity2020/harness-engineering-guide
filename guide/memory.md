# Memory Systems

## The Memory Problem

LLMs have no persistent memory. Every API call starts from zero. The harness is responsible for giving agents the *illusion* of continuity — and ideally, real continuity.

## AGENTS.md / MEMORY.md Pattern

This file-based pattern has emerged as a de facto standard across multiple harness implementations:

### File Structure

```
workspace/
├── AGENTS.md          # Agent identity, rules, configuration
├── MEMORY.md          # Curated long-term memories
├── memory/
│   ├── 2026-04-13.md  # Today's raw session log
│   ├── 2026-04-12.md  # Yesterday's log
│   └── ...
├── TOOLS.md           # Tool-specific notes (API keys, device names)
└── SOUL.md            # Agent personality & values (optional)
```

### How It Works

1. **Every session start:** Agent reads AGENTS.md + MEMORY.md + recent daily logs
2. **During session:** Agent appends to today's `memory/YYYY-MM-DD.md`
3. **Periodically:** Agent reviews daily logs and distills insights into MEMORY.md
4. **Result:** MEMORY.md becomes curated wisdom; daily logs are raw data

### Why Plain Text?

- **Human-readable** — Users can inspect and edit their agent's memory
- **Version-controlled** — Memory changes are tracked in git
- **Portable** — Move between harnesses by copying files
- **No vendor lock-in** — No proprietary database format

## Session vs Long-term Memory

| | Session Memory | Long-term Memory |
|--|---------------|-----------------|
| **Lifespan** | Single conversation | Across all sessions |
| **Storage** | Context window + temp files | MEMORY.md + vector DB |
| **Managed by** | Context manager | Memory consolidation agent |
| **Capacity** | Limited by context window | Unlimited (with retrieval) |

## Memory Ownership & Portability

The most consequential question in agent memory design:

### User-Owned (Open)
- Memory stored as local files (MEMORY.md, daily logs)
- User can read, edit, delete, and export
- Portable between platforms
- **Example:** OpenClaw, Nexu

### Platform-Owned (Closed)
- Memory stored on vendor servers
- Opaque format, no export
- Lost if you leave the platform
- **Example:** Claude's "memory" feature, Codex encrypted summaries

### Hybrid
- Platform hosts memory, but user can export
- API access to memory contents
- **Example:** Some enterprise agent platforms

---

*Next: [Security & Sandboxing →](security.md)*
