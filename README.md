
https://github.com/user-attachments/assets/12ee4bcb-6d1c-496d-a70e-a5a1578a879e





# VAINOM - Persistent Cognitive Engine
[**⬇ Download VAINOM 0.5.8**](https://github.com/DVirdis/VAINOM/releases/latest) | [**🌐 vainom.com**](https://vainom.com)

Welcome to the VAINOM Open Beta.

This is not another chat app with a disconnected vector database bolted on. VAINOM runs entirely on your machine - your GPU, your disk, your rules - and it keeps a real memory across sessions. Close it, wipe every chat, delete every document you ever gave it, and it still remembers you. The knowledge lives in the brain, not in the logs.

**97.4% session recall on LongMemEval-S (470 questions, Session Recall@5).** That is the number. Not a marketing one. Measured the same way the official benchmark measures it. Which is why we are not afraid to put it first.

On HotPotQA (200 hard multi-hop questions), VAINOM retrieved **100% of the supporting evidence at Recall@30**, **86.5% at Recall@5**, in **56.7 ms** average query time, on an RTX 4060 laptop - not a datacenter GPU.

As far as we know, no fully local system running on consumer hardware has published comparable numbers.

---

## Requirements

- **OS:** Windows 10 / 11. Linux and macOS are in the later roadmap.
- **GPU:** NVIDIA (CUDA) or AMD (Vulkan), **8 GB VRAM minimum** for the default model
- **RAM:** 16 GB minimum
- **Disk:** ~6 GB for the default model, plus whatever you feed it

Mobile companion app: **Android only** for now.

---

## Quick start

1. Unzip wherever you like. Keep the folder structure intact.
2. Double-click `VAINOM.exe`.
3. First launch takes a moment while the engine initializes and loads the default model. Give it a few seconds.
4. The window opens. On the right sidebar, click **MODELS -> SHOW** - you should see the default chat model listed as **ACTIVE**. If not, click **LOAD**, pick the one in `vainom_system/models/`, then click it to activate.
5. Open **PREFERENCES** (top bar). Set your **Display Name**, **Language**, and optionally a **Custom Instruction** describing how you want VAINOM to behave.
6. Say hi. Introduce yourself. Ask something light.

That is it. No account, no login, no telemetry, no "please accept cookies."

---

## Formative phase - please read this

VAINOM ships as a blank slate. The brain is empty the first time you run it. The sidebar will show your **STAGE** starting at **INFANT**, **LEVEL 1**, and a handful of nodes, edges, and episodes.

**It grows with use.** The more you talk to it, the better it becomes at recalling and linking what matters to you. This is a feature, not a limitation - it is how the memory becomes *yours* and not some generic pre-trained persona.

During the first hours / days:

- **Start light.** Introduce yourself. Tell it your name, what you do, what you like. Let it ask back.
- **Don't firehose it.** Dumping a 200-page PDF and ten years of personal history on day one is the fastest way to make it look dumb. It has not yet learned what matters to *you*. Early on, signal gets drowned in volume.
- **Expect the occasional "I don't remember that yet."** Early memory is sparser and noisier. It settles.

Think of it as a child with a very good long-term memory and no short-term context yet. You are not using a product. You are raising one.

After a week or two of normal use, you will notice the shift. It stops asking the same things. It brings up stuff from old chats unprompted and correctly. It connects things you never explicitly linked. That is the point at which VAINOM stops being a chatbot and starts being *yours*.

---

## What VAINOM can do (the tools)

VAINOM decides on its own when to use memory, document, web, browsing, or scheduling tools. Here is what it has at its disposal:

- **Memory recall** (`TOOL_RECALL`) - searches its own long-term memory for anything related to what you are discussing. Filters by who said it (user / self / document) and how many hits to pull.
- **Memory store** (`TOOL_STORE`) - extracts atomic facts from the conversation and commits them to the graph. This is the mechanism behind persistent memory: every exchange can leave a trace in the form of subject-relation-object triples.
- **Document search** (`TOOL_DOC`) - searches across every document you have ingested via the **DOCS -> ADD** button. Can target a specific file, a specific page, or run free-text across all of them.
- **Document read** (`TOOL_DOC_READ`) - sequentially reads chunks of a document when it needs full context rather than a match.
- **Web search** (`TOOL_WEB`) - searches the internet and fetches pages when it needs current information. Also accepts a direct URL.
- **Headless browser** (`TOOL_BROWSE`) - when a page is JavaScript-heavy and plain fetch won't work (prices, dynamic content, dashboards), VAINOM opens it in a real browser and reads what a human would see.
- **Scheduler** (`TOOL_SCHEDULE`) - reminders and events. Say "remind me on Friday at 9 to call Marco" and it books it. Visible in the **CALENDAR** panel on the left.

---

## The Brain

Click the **BRAIN** button (top left) to see all the connections VAINOM made. **BRAIN SIZE** in the top right shows how much space the whole memory occupies - usually a fraction of what a single photo takes.

**IMPORT** and **EXPORT** let you back up or transfer the brain. The brain file is the "soul" of your VAINOM. Guard it, copy it, carry it around. It is yours.

Deleting a chat or a document does **not** erase what VAINOM learned from it. The brain keeps the distilled knowledge, not the raw transcript. This is intentional.

---

## Swapping the chat model

The default chat model is tuned to work well on 8 GB VRAM. If you have more, you can run bigger.

1. Download any GGUF chat model (the usual places: Hugging Face, etc.)
2. Drop it into `VAINOM/vainom_system/models/`
3. In VAINOM: **MODELS -> SHOW**, click **DISCARD** on the current one, then **LOAD**, pick the new file, click it to activate.

The embedder (the component that turns text into vectors) is a different story.

**We strongly recommend leaving the default embedder alone.** The brain is built against it - swap it and old memories lose coherence.

If you really know what you are doing and want to experiment: place a GGUF embedder in `vainom_system/models/` with the word `embed` in the filename (e.g. rename Snowflake to `embed_snowflake.gguf`). Only one embedder can live in that folder at a time. The system picks it automatically by name. Do this on a fresh brain, not on an existing one.

---

## Connecting to Claude Desktop (MCP)

If you don't want to stay fully offline, this is one of VAINOM's killer features.

VAINOM ships with a built-in MCP (Model Context Protocol) server that exposes **9 tools** Claude Desktop can call directly: query and write into your memory, read your documents, save triples, persist the brain, and more. Claude stops being a stateless chatbox. Your VAINOM brain becomes its long-term memory.

1. In VAINOM sidebar: click **MCP -> START**. The indicator walks through **START -> READY -> CONNECTED** as the handshake completes.
2. In Claude Desktop, open Settings -> Developer -> Edit Config, and follow Claude's own instructions for adding a local MCP server. The bridge is already inside `VAINOM.exe` - you don't need to run anything extra.
3. Restart Claude Desktop.

From that moment Claude can read, query, and write into your VAINOM brain. Your memory, shared across tools, still living only on your machine.

---

## Mobile (Android)

Your phone becomes a frontend. The brain stays home.

1. Click **NETWORK** in the top bar and switch **Remote access** to **ON**. VAINOM will try to open the port on your router automatically (UPnP). If that works, you are done. If it fails, you will need to forward the port manually on your modem - follow the in-app instructions.
2. Click **MOBILE** in the sidebar. A QR code appears with two options: **LAN** (same Wi-Fi as your PC) or **REMOTE** (anywhere in the world).
3. Open the VAINOM Android app, scan the QR, done.

The link between phone and PC is end-to-end encrypted. Nobody sitting between your phone and your PC sees what you say or what VAINOM answers.

---

## Privacy

- **No cloud inference. No telemetry. No accounts. No phone-home.** The model runs on your GPU, the brain lives on your disk.
- **Four things can cross the boundary of your machine, and only when you explicitly enable them:**
  - **Web search** - runs through DuckDuckGo Lite over pure HTTPS. No JavaScript, no cookies, no trackers, no ad pixels.
  - **Web browse** - when a page needs JavaScript to render, VAINOM opens it in an isolated headless session. Cookies, cache, localStorage and sessionStorage are wiped **before every single navigation**. Trackers and ad pixels are blocked at the network level (requests to tracker domains are intercepted and dropped). User-Agent is randomized per session. Nothing is persisted, nothing follows you across pages, nothing touches your regular browser profile.
  - **Mobile pairing** - your phone talking to your own PC. End-to-end encrypted. LAN stays on your Wi-Fi; REMOTE passes through the open internet but the payload is sealed - neither us nor your ISP can read it.
  - **MCP** - the MCP bridge itself is local to your machine. What the connected app does with retrieved data depends on that app's own configuration and behavior.
- **The brain file lives on your disk.** Back it up yourself. It's yours.

---

## Feedback and bugs

- Email: **hello@vainom.com** 
- Instagram: **@vainom.exe**

Tell us what broke, what surprised you, what made you uncomfortable, what you wish it did. This is an open beta. Your rough edges are our roadmap.

---

## License

Proprietary. All rights reserved.

---

*VAINOM starts at stage INFANT and grows. Be patient with it at first. It will surprise you soon enough.*
