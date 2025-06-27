# open-memory-object
Open Memory Object (OMO) - Universal standard for AI memory interoperability with built-in safety, privacy, and consent management. Enabling seamless memory portability across platforms, databases, and AI systems while prioritizing user sovereignty and frontier AI safety.

# Open Memory Object (OMO)

**Universal Standard for AI Memory Interoperability**

[![Status](https://img.shields.io/badge/status-in%20formation-yellow)](https://github.com/papr-ai/open-memory-object)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue)](LICENSE)
[![Discord](https://img.shields.io/badge/discord-omo--spec-7289da)](https://discord.gg/omo-spec)

> **🚨 Status:** This consortium is actively forming. The specification has not yet been ratified by founding members.

## Why OMO?

LLM-powered agents and AI applications depend on **long-term memory** for personalized experiences. Today, that memory is **fragmented** across proprietary formats, creating:

- **Platform Lock-in** - OpenAI, Anthropic, Google store memory in closed schemas
- **Integration Tax** - Developers build bespoke adapters for each memory platform  
- **Data Sovereignty Loss** - Users can't export, audit, or delete their memories
- **Safety Gaps** - Inconsistent consent, risk assessment, and content filtering
- **Innovation Drag** - Startups duplicate infrastructure instead of building features

**OMO solves this** with a universal, safety-first standard for AI memory objects.

## Core Principles

### 🛡️ **Safety First**
Every memory object carries built-in safety metadata, enabling automatic risk assessment and content filtering before reaching AI models.

### 👤 **User Sovereignty** 
Memory belongs to users, not platforms. All operations must respect explicit consent and provide "forget-me" controls.

### 🔒 **Privacy by Design**
Consent tracking is mandatory. Every memory object explicitly records how consent was obtained and permissions granted.

### 🔗 **Universal Interoperability**
One standard across memory platforms, databases, cloud providers, and AI systems. All specifications, reference implementations, and governance processes are publicly available under permissive licenses.

### ⚡ **Developer Simplicity**
Single integration point reduces complexity and accelerates innovation.

## Schema Overview

```json
{
  "id": "mem_abc123",
  "createdAt": "2025-06-26T10:30:00Z",
  "type": "text",
  "content": "User prefers morning meetings",
  "consent": "explicit",
  "risk": "none",
  "topics": ["scheduling", "preferences"],
  "acl": {
    "read": ["user:alice", "app:calendar"],
    "write": ["user:alice"]
  },
  "ext": {
    "papr:relationships": [...],
    "qdrant:vector": [0.1, 0.2, ...]
  }
}

