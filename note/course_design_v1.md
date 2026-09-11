# Prodsup 69 — Course Improvement Report

**255217 / 255411 · CMU-NR · Academic Year 69**  
_Evidence-based recommendations drawn from 27 lecture transcripts and 22 slide decks from Year 68._

---

## Framework: The ISA-95 Spine — Keep It

The pyramid is the best thing about this course's architecture. Every lecture in Year 68 was anchored to it, and the transcripts show students absorbing it naturally. Don't change the structure — deepen the connections within it.

| Level                | Scope                  | Tools (Year 68)                                     |
| -------------------- | ---------------------- | --------------------------------------------------- |
| L4 — ERP             | Business planning      | ERPNext (Purchase, Sales, Inventory, Manufacturing) |
| L3 — MES / SCADA     | Production management  | Node-RED, InfluxDB, Dashboard, Telegram             |
| L2 — Supervision     | Monitoring & control   | Node-RED flows, MQTT broker                         |
| L1 — Control         | PLCs, local automation | (conceptual only)                                   |
| L0 — Field / Sensors | Physical devices       | ESP32 sensors, MQTT publish                         |

> **Key insight from ps68_06:** You explicitly describe Node-RED as _"ระบบที่มันทำหน้าที่คล้ายๆ กับระบบ SCADA"_ — lightweight SCADA filling the gap between ERP and the shop floor. This framing is the course's core insight and should carry into Year 69 unchanged.

---

## Key Observations from Transcripts

### Strengths

**Strong "Why Before How"** — Every session opens with the industrial problem before touching software. Students are told why MQTT beats HTTP for sensors, why InfluxDB beats a relational DB for time-series. This is exceptional pedagogy — keep it.

**Business Cycle Completeness** — The project rubric ties ERPNext documents (Purchase Order → Sales Order → Work Order → Stock Entry) to Node-RED sensor data. Students graduate understanding a full manufacturing cycle, not just isolated tools.

**MQTT Explanation is Exemplary** — The MQTT vs. HTTP contrast in ps68_08 is the clearest teaching of this concept I've seen. QoS levels, retained messages, and wildcards are all covered with concrete rationale.

**Image Classification is a Highlight** — Teachable Machine + ml-express is original, hands-on, and memorable. Students build and deploy their own model. Object Detection (DETR) is correctly framed as optional.

**Integration Lessons Are the Core** — ps68_19 (Basic: HTTP GET/POST) and ps68_20 (Advanced: Work Order pull → Stock Entry push) are the most industrially relevant content in the course.

### Areas for Improvement

**InfluxDB — First Year Depth** — ps68_14 confirms this was the first year on InfluxDB (previously Firebase). Coverage was correct but surface-level. Flux queries, retention policies, and dashboard integration remain untapped for Year 69.

**Self-Hosting Setup Complexity** — T31 (pnpm + nvm + Chocolatey) and T22–T23 (LocalXpose CLI) introduce significant Windows-specific friction. Multiple tool chains will consume disproportionate lab time.

**IoT Sensor WiFi Pain Points** — ps68_12 reveals that iPhone 2.4 GHz Hotspot setup (Maximize Compatibility, exact SSID matching) is a recurring student hurdle. A pre-configured lab router would eliminate this class of problem.

**Context Lectures Fragmented** — Node-RED Context is split across three separate videos (Basics → Counter → Selection). One focused session would serve students better.

---

## Existing Topics — Keep, Improve, or Cut

| Topic                                               | Action              | Notes                                                                              |
| --------------------------------------------------- | ------------------- | ---------------------------------------------------------------------------------- |
| Introduction — ISA-95 Pyramid                       | **Keep**            | Add one slide showing UNS/MQTT placement as a course preview                       |
| ERPNext — Purchase, Sales, Inventory, Manufacturing | **Keep**            | Update screenshots if ERPNext version changed; content structure is right          |
| MQTT — Theory, Broker, Topics, QoS, Retained        | **Keep + Extend**   | Add ISA-95 topic hierarchy example (`factory/line/device/metric`)                  |
| Node-RED Dashboard (@flowfuse)                      | **Improve**         | Keep for real-time display; add Grafana as the professional historical alternative |
| InfluxDB Cloud                                      | **Improve**         | Add Flux query syntax, retention policies, Grafana connection                      |
| Telegram Notification                               | **Keep**            | Simple, effective, immediately satisfying — no changes needed                      |
| IoT Sensor (ESP32 → MQTT)                           | **Keep + Fix**      | Use a dedicated lab router (fixed SSID, 2.4 GHz) to eliminate hotspot issues       |
| Node-RED Context (3 videos)                         | **Consolidate**     | Merge into one 60-minute session: scope → counter → selection demo                 |
| ERPNext Integration — Basic                         | **Keep**            | Optionally add a debug-node REST test step before ERPNext                          |
| ERPNext Integration — Advanced                      | **Keep + Extend**   | Add physical sensor trigger (button/counter) for the Stock Entry                   |
| Self-Hosting (pnpm + nvm)                           | **Replace**         | Switch to Docker Compose — simpler, more reproducible, more industry-relevant      |
| Local Tunnel (LocalXpose)                           | **Switch**          | Replace with Cloudflare Tunnel (`cloudflared`) — free, well-documented             |
| Image Classification (Teachable Machine)            | **Keep**            | Add cross-group model testing to reveal overfitting                                |
| Object Detection (DETR)                             | **Keep (Optional)** | Mention YOLOv8/Ultralytics as a more current alternative                           |

---

## Proposed New Modules

All constrained to open-source software and consumer-grade hardware.

### 🔵 Grafana — Industrial Dashboard (~2 hrs, Medium)

Grafana is the most common open-source monitoring tool in real factories. Connect it to the existing InfluxDB setup; students build a production-ready time-series dashboard that no Node-RED dashboard can match visually. Also de-risks the course against InfluxDB Cloud pricing changes — Grafana + local InfluxDB is fully self-hostable.

### 🔵 Docker Compose — Stack Deployment (~2 hrs, Medium)

Replace the pnpm/nvm self-hosting session with a `docker-compose.yml` that brings up Node-RED + InfluxDB + Grafana + Mosquitto in one command. Students type `docker compose up -d` and everything is running. This is the actual way IoT stacks are deployed at the edge in 2026, and it eliminates most Windows-specific setup pain.

### 🔵 Unified Namespace (UNS) Concept (~1 hr, Low)

Extend the MQTT session 30–45 minutes to introduce UNS: structuring MQTT topics as a hierarchy that mirrors ISA-95 (`enterprise/site/area/line/device/metric`). Students already know MQTT — this is just teaching them to name topics correctly, with zero implementation cost but high industry impact.

### 🔵 OEE Dashboard — Capstone Integration (~2 hrs, Medium)

Overall Equipment Effectiveness (OEE = Availability × Performance × Quality) is the KPI every factory tracks. Build a simple OEE calculation flow in Node-RED that pulls Work Order data from ERPNext, counts sensor events, writes results to InfluxDB, and displays in Grafana. Ties all course tools together in one realistic use-case — a strong portfolio piece.

### 🔵 LLM Integration — Smart Alerts (~1 hr, Low)

Use a Node-RED `HTTP Request` node to call a local Ollama instance with sensor readings and get a natural-language maintenance recommendation. Students already know the HTTP Request node from the ERPNext integration lesson. Frame as a demo/teaser, not a graded requirement.

### 🔵 Edge AI Teaser — TinyML on ESP32 (~1 hr, Low, demo only)

Show a pre-trained Edge Impulse model running inference directly on an ESP32 — no cloud, no server. Positions this as "Image Classification on the sensor itself." Students don't train the model; they just see it run. Edge Impulse supports ESP32 natively; uses hardware students already have.

---

## Proposed Year 69 Syllabus

| Session | Topic                                                    | Change from Year 68                                    | ISA-95 |
| ------- | -------------------------------------------------------- | ------------------------------------------------------ | ------ |
| T01     | Introduction — ISA-95, Course Overview                   | Add UNS preview slide                                  | All    |
| T02     | ERPNext — Accounting & Purchase                          | Unchanged                                              | L4     |
| T03     | ERPNext — Sales & Inventory                              | Unchanged                                              | L4     |
| T04     | ERPNext — Manufacturing (BOM → Work Order)               | Unchanged                                              | L4     |
| T05     | Node-RED Introduction                                    | Unchanged                                              | L3     |
| T06     | MQTT — Theory, Topics, QoS + **UNS Topic Hierarchy**     | **Extend** with ISA-95 topic naming                    | L0–L3  |
| T07     | Node-RED Dashboard (@flowfuse)                           | Unchanged                                              | L3     |
| T08     | Telegram Notification                                    | Unchanged                                              | L3     |
| T09     | IoT Sensor (ESP32 → MQTT)                                | Fix WiFi docs; use lab router                          | L0     |
| T10     | Node-RED Context — Node / Flow / Global                  | **Consolidate** 3 videos → 1 session                   | L3     |
| T11     | InfluxDB — **Flux Queries + Retention Policies**         | **Deepen** Year 68 basics                              | L3     |
| T12     | **Grafana — Connect to InfluxDB, Build Dashboard**       | **NEW** — replaces one LocalXpose session              | L3–L4  |
| T13     | ERPNext Integration — Basic (HTTP GET/POST)              | Unchanged                                              | L3–L4  |
| T14     | ERPNext Integration — Advanced (Work Order, Stock Entry) | Add physical sensor trigger                            | L3–L4  |
| T15     | **Docker Compose — Self-Hosting Full Stack**             | **NEW** — replaces pnpm/nvm session                    | L3     |
| T16     | Local Tunnel — **Cloudflare Tunnel**                     | Switch from LocalXpose to `cloudflared`                | L3     |
| T17     | Image Classification (Teachable Machine + ml-express)    | Add cross-group testing exercise                       | L3     |
| T18     | Object Detection (DETR) — Optional                       | Mention YOLOv8 as modern alternative                   | L3     |
| T19     | **OEE Dashboard — Capstone Integration Exercise**        | **NEW** — ties ERPNext + Node-RED + InfluxDB + Grafana | All    |
| T20     | **LLM / Edge AI Teaser — Demo Session**                  | **NEW** — Ollama demo + Edge Impulse ESP32 demo        | L0/L3  |
| T99     | Project — Briefing, Q&A, Scheduling                      | Update rubric                                          | —      |

---

## Suggested Year 69 Project Rubric

Year 68 was 35 raw points = 20% of grade. Proposed rubric: **40 raw points = 20% of grade.**

| Category           | Points | Notes                                                                     |
| ------------------ | ------ | ------------------------------------------------------------------------- |
| Proposal           | 3      | Title, scope, members, timeline — unchanged                               |
| ERPNext            | 8      | Item master, BOM, Work Order, Purchase/Sales flow (+1 from Y68)           |
| Node-RED + IoT     | 10     | MQTT ingestion, dashboard, sensor, Telegram alert (−2 from Y68)           |
| InfluxDB + Grafana | 4      | **NEW** — time-series storage + Grafana visualization panel               |
| ERP Integration    | 6      | Node-RED ↔ ERPNext: GET work order + POST stock entry                     |
| Extras             | 4      | Any of: Image Classification, Object Detection, OEE, LLM, Docker, Edge AI |
| Presentation       | 5      | Live demo + Q&A — unchanged                                               |
| **Total**          | **40** |                                                                           |

Adding 5 raw points gives more resolution at the top end — strong groups can earn all 40 without needing the Extras category, while weaker groups still pass with the core stack.

---

## What to Do First

1. **Write the Docker Compose file** _(High Priority)_ — One `docker-compose.yml` running Node-RED + InfluxDB + Grafana + Mosquitto. Foundation for self-hosting, Grafana, and OEE capstone — eliminates Windows setup friction in one move.

2. **Build the Grafana + InfluxDB lesson** _(High Priority)_ — Students already have InfluxDB data from Year 68. One new session connecting Grafana and building a production-grade panel is low-effort, high-impact.

3. **Add ISA-95 topic hierarchy to the MQTT lecture** _(Quick Win)_ — One extra slide + live demo renaming MQTT topics to `factory/line/device/metric`. No new tools, 30-minute extension, industry-level impact.

4. **Consolidate the Context lectures** _(Medium Priority)_ — Merge three Context videos into one structured session. Use the freed slot for the OEE exercise or Grafana lesson.

5. **Procure a dedicated lab router** _(Infrastructure)_ — Fixed SSID and 2.4 GHz band removes the single biggest source of repeated lab support issues permanently.

6. **Prepare the LLM + Edge AI demo session** _(Future Signal)_ — Install Ollama on the demo laptop. Pull a small model (`llama3.2:1b` or `qwen2.5:1.5b`). Prepare one Node-RED flow that sends sensor readings and gets back a maintenance recommendation.

---

## OPC-UA — Considered and Deferred to Year 70

### The Case For It

OPC-UA sits squarely in the ISA-95 L1/L2 gap the course already frames well. In real factories it is the protocol PLCs and SCADA systems use to talk to each other — the industrial network layer before the internet. Students who know MQTT (lightweight, pub/sub, IoT-native) _and_ understand OPC-UA (rich address space, standardized data model, built-in security, request/response + subscriptions) have the full L0–L3 communication picture. `node-red-contrib-opcua` is mature enough to run a Node-RED OPC-UA server without significant overhead.

### The Consumer Problem

OPC-UA's value proposition is **interoperability** — any conformant client can connect. But demonstrating that requires an industrial client. The options considered:

| Option                                 | Verdict                                                                                                                                                                                                                                                     |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Ignition Maker Edition**             | Too heavy. Full SCADA platform — gateway, designer, tag DB. Confuses the narrative (Node-RED _is_ the lightweight SCADA; adding Ignition undermines that). 8-hour trial resets are manageable but the learning curve is the real cost. **Not recommended.** |
| **UAExpert** (Unified Automation)      | Free, lightweight OPC-UA browser. Good for showing the address space and live subscriptions. Demonstrates interoperability without introducing a second application platform. Best option _if_ OPC-UA is added.                                             |
| **Node-RED as both client and server** | Keeps toolset contained; client subscribes and pushes into InfluxDB → Grafana. Pedagogically weak — if client and server are both Node-RED, students don't feel the interoperability benefit. Looks like MQTT with more ceremony.                           |
| **Grafana OPC-UA plugin / Telegraf**   | Technically valid (Telegraf OPC-UA input → InfluxDB → Grafana), but Grafana plugin is still maturing and adds Telegraf as another component.                                                                                                                |

### Decision: Add to T01 as Context; Defer Full Lab to Year 70

**Year 69 action:** Add one slide to T01 Introduction positioning OPC-UA in the ISA-95 diagram — show it as the L1/L2 protocol students will encounter in real factories, contrast it briefly with MQTT, and explain why MQTT is the right teaching tool for this course. No lab, no installation, no graded requirement.

Suggested framing for the slide: _"MQTT is what we use in this course — open, lightweight, perfect for IoT devices. OPC-UA is what you will find when you walk into a factory after graduation — same communication idea, but with a richer data model standardized by IEC 62541. Node-RED can speak both."_

**Year 70 plan:** Once Docker/Grafana/UNS additions from Year 69 are settled, add a proper OPC-UA session:

1. Node-RED as OPC-UA server (`node-red-contrib-opcua`) — expose sensor variables as an address space
2. UAExpert as browser demo — students point it at the server, browse the node tree, subscribe to live values
3. Node-RED as OPC-UA client — subscribe and feed data into InfluxDB → Grafana pipeline

This sequence gives OPC-UA a full session with a compelling consumer demo, without overloading Year 69.

---

_Generated from review of ps68_01–ps68_28 transcripts + T01–T99 slides · nirand.p@cmu.ac.th · Sep 2026_
