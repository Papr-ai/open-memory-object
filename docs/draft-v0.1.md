<figure data-type="blockquoteFigure">
<div><blockquote><p data-id="fcbe17b7-86a0-465b-93b4-31dbc95e4521"><strong>Status:</strong> ✨ <em>Working draft for community review.</em><br>This document <strong>has not yet been ratified</strong> by the founding members (Georgia Tech, Qdrant, etc.).<br>Comments, Pull&nbsp;Requests, and Issues are welcome.</p></blockquote><figcaption></figcaption></div>
</figure>

---

## 1  Why OMO?

LLM–powered agents, assistants, and applications depend on **long‑term memory** to deliver personalized, context‑rich experiences. Today that memory is **fragmented** across proprietary formats:

- **Platform Lock‑in.** OpenAI, Anthropic, Google, and others each store user memory in closed schemas.

- **Integration Tax.** Devs integrate the same app with Papr, Mem0, Zep, or Letta  via bespoke adapters.

- **Data Sovereignty Loss.** Users cannot readily export, audit, or delete their own memories.

- **Safety Gaps.** Consent, risk flags, and content filtering differ wildly by vendor.

- **Innovation Drag.** Start‑ups duplicate plumbing instead of building new capability.

- **Vendor Dependency**: Switching AI providers means losing months or years of personalized context

The result: higher switching costs, slower progress—and users increasingly lose control of their data.

---

## 2  Guiding Principles

#### **1. User Sovereignty**

Memory belongs to the user, not the platform. Users should have explicit control over how their memories are stored, shared, and used across AI systems. All operations must respect explicit consent and a hard "forget‑me" switch.

#### **2. Safety First**

Every memory object carries built-in safety metadata, enabling automatic risk assessment and content filtering before it reaches AI models.

#### **3. Privacy by Design**

Consent tracking is mandatory, not optional. Every memory object explicitly records how consent was obtained and what permissions were granted.

#### **4. Interoperability Without Compromise**

Standardization shouldn't mean lowest-common-denominator. The extension system allows platform-specific features while maintaining core compatibility.

#### **5. Developer Simplicity**

Reducing integration complexity accelerates innovation. One standard should work across memory layers, databases, cloud providers, and AI platforms.

---

## 3  Goals

1. **Cross-Platform Memory Portability**

   - **Memory Platforms**: Papr, Mem0, Zep, Letta, Pinecone Memory
   - **Databases**: Neo4j, Qdrant, MongoDB, PostgreSQL, Supabase, Neon
   - **Cloud Providers**: Azure, AWS, Google Cloud
   - **LLM Providers**: OpenAI, Anthropic, Google, local models
   - **Applications**: Replit, Lovable, Cursor, Notion AI, GitHub Copilot, custom agents
   - **Orchestration**: LangChain, LangGraph, Letta, custom frameworks

2. **Built-in Safety & Security**

   - Automatic risk assessment for sensitive content
   - Granular access controls (read/write permissions)
   - Content filtering capabilities
   - Audit trails for compliance

3. **Privacy & Consent Management**

   - Explicit consent tracking for every memory object
   - GDPR/CCPA compliance ready
   - User-controlled data retention policies
   - "Right to be forgotten" implementation

4. **Developer Experience**

   - Single integration point for multiple platforms
   - Extensible schema for platform-specific features
   - RESTful APIs with consistent behavior
   - Reference implementations and SDKs

---

## 4  Architecture at a Glance

```javascript
          +--------------------+
          |    AI Application  |
          +---------+----------+
                    |
          +---------v----------+
          |   OMO API Layer    |  <-- spec + reference impl
          +---------+----------+
            |   |   |   |
  Memory Lyrs  DBs  Clouds  LLMs
```

---

## 5  Draft Schema (stable core)

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id":     "https://open-memory.org/OMO-v1.schema.json",
  "title":   "Open Memory Object (OMO)",
  "type":    "object",

  "required": ["id", "createdAt", "type", "content", "consent"],
  "properties": {
    "id":        { "type": "string", "description": "Global URI or UUID" },
    "createdAt": { "type": "string", "format": "date-time" },

    "type": {
      "type": "string",
      "enum": ["text", "image", "audio", "video", "file", "code"],
      "description": "Primary media type of the memory"
    },

    "content":   { "type": "string", "description": "UTF-8 (or base64) body—may be truncated for binary blobs" },

    "consent": {
      "type": "string",
      "enum": ["explicit", "implicit", "terms", "none"],
      "description": "How the data owner allowed this memory to be stored/used"
    },

    "risk": {
      "type": "string",
      "enum": ["none", "sensitive", "flagged"],
      "default": "none",
      "description": "Post‑ingest safety assessment: suppress or filter on retrieval"
    },

    "topics":    { "type": "array", "items": { "type": "string" } },
    "sourceUrl": { "type": "string", "format": "uri" },

    "acl": {
      "type": "object",
      "properties": {
        "read":  { "type": "array", "items": { "type": "string" } },
        "write": { "type": "array", "items": { "type": "string" } }
      },
      "additionalProperties": false
    },

    "ext": {
      "type": "object",
      "description": "Namespaced extension fields; keys SHOULD use a vendor prefix (e.g. 'papr:relationships')",
      "additionalProperties": true
    }
  },

  "additionalProperties": false
}
```

---

## 6  Mapping to Community Requests

<table data-id="14c770de-1df5-47f8-b6bf-3e770e808168" style="min-width: 50px">
<colgroup><col style="min-width: 25px"><col style="min-width: 25px"></colgroup><tbody><tr><td colspan="1" rowspan="1"><p data-id="e7b8b8f2-09f9-4d43-b91b-2f95ac55f888"></p></td><td colspan="1" rowspan="1"><p data-id="41ad0950-a073-450d-bdae-5c9378c872e6">Requirement OMO&nbsp;Field / Plan</p></td></tr><tr><td colspan="1" rowspan="1"><p data-id="de81e9f5-a41c-45ec-9bd6-a6c8ad7f9926"><strong>Memory Schema</strong></p></td><td colspan="1" rowspan="1"><p data-id="424ba1a5-de2e-44ba-9d70-004b16c9a3a6">JSON above (stable core +&nbsp;<code>ext</code>)</p></td></tr><tr><td colspan="1" rowspan="1"><p data-id="64906e87-3be2-429c-a420-a790ab6168e4"><strong>Embedding/Serialization Wrapper</strong></p></td><td colspan="1" rowspan="1"><p data-id="04414a42-6314-4c8e-87e8-d204a97e6c9c">Recommendation to store vectors in <code>ext</code> (e.g. <code>qdrant:vector</code>) for transparency</p></td></tr><tr><td colspan="1" rowspan="1"><p data-id="d65733db-9fec-48ad-bd1e-2b0313280fff"><strong>CRUD&nbsp;API</strong></p></td><td colspan="1" rowspan="1"><p data-id="ad017531-47d7-465a-adf6-77824bae8a28">Reference REST endpoints (<code>/omo</code>&nbsp;POST&nbsp;/ GET&nbsp;/ PATCH&nbsp;/ DELETE) in upcoming RFC‑002</p></td></tr><tr><td colspan="1" rowspan="1"><p data-id="0957cd5b-e7d1-4d89-835b-1ec9804da9e7"><strong>Permission Model</strong></p></td><td colspan="1" rowspan="1"><p data-id="c3a5aa53-c854-4e39-9305-33cc6bd98f7a"><code>consent</code>, <code>acl</code>, <code>risk</code>, and TTL (<code>ext:ttl</code>)</p></td></tr><tr><td colspan="1" rowspan="1"><p data-id="917d6849-3e64-48ae-8c49-44b938993a23"><strong>Interchange File</strong></p></td><td colspan="1" rowspan="1"><p data-id="aeb75b7b-3be8-4488-ae75-7f14c1547c4d"><code>.omo.json</code> export = array of OMO objects</p></td></tr></tbody>
</table>

---

## 7  Governance & Roadmap

- **Founding Members (in formation):** Papr, Georgia Tech CoC, Qdrant (seeking Neo4j, Chroma, Mem0…)

- **July 2025:** Kick‑off WG‑1 (Schema), WG‑2 (API), WG‑3 (Safety Tests)

- **Q3 2025:** Publish Reference SDKs (TS, Python), certification test harness.

---

## 8  How to Contribute

1. Open an Issue or PR.

2. Join #omo‑spec on Discord.

3. Sign the lightweight **Contributor Agreement**.

---

## 9  License

Released under the **Apache 2.0** license.

---

© 2025 Open Memory Consortium (draft)
