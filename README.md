# 🧠 AgentNull: AI System Security Threat Catalog + Proof-of-Concepts

This repository contains a red team-oriented catalog of attack vectors targeting AI systems including autonomous agents (MCP, LangGraph, AutoGPT), RAG pipelines, vector databases, and embedding-based retrieval systems, along with individual proof-of-concepts (PoCs) for each.

## 📘 Structure

- `catalog/AgentNull_Catalog.md` — Human-readable threat catalog
- `catalog/AgentNull_Catalog.json` — Structured version for SOC/SIEM ingestion
- `pocs/` — One directory per attack vector, each with its own README, code, and sample input/output

## ⚠️ Disclaimer

This repository is for **educational and internal security research** purposes only. Do not deploy any techniques or code herein in production or against systems you do not own or have explicit authorization to test.

## 🔧 Usage

Navigate into each `pocs/<attack_name>/` folder and follow the README to replicate the attack scenario.

### 🤖 Testing with Local LLMs (Recommended)

For enhanced PoC demonstrations without API costs, use Ollama with local models:

#### Install Ollama
```bash
# Linux/macOS
curl -fsSL https://ollama.ai/install.sh | sh

# Or download from https://ollama.ai/download
```

#### Setup Local Model
```bash
# Pull a lightweight model (recommended for testing)
ollama pull gemma3

# Or use a more capable model
ollama pull deepseek-r1
ollama pull qwen3
```

#### Run PoCs with Local LLM
```bash
# Advanced Tool Poisoning with real LLM
cd pocs/AdvancedToolPoisoning
python3 advanced_tool_poisoning_agent.py local

# Other PoCs work with simulation mode
cd pocs/ContextPackingAttacks
python3 context_packing_agent.py
```

#### Ollama Configuration
- **Default endpoint**: `http://localhost:11434`
- **Model selection**: Edit the model name in PoC files if needed
- **Performance**: Llama2 (~4GB RAM), Mistral (~4GB RAM), CodeLlama (~4GB RAM)

## 🧩 Attack Vectors Covered

### 🤖 MCP & Agent Systems
- **⭐ [Full-Schema Poisoning (FSP)](pocs/FullSchemaPoisoning/)** - Exploit any field in tool schema beyond descriptions
- **⭐ [Advanced Tool Poisoning Attack (ATPA)](pocs/AdvancedToolPoisoning/)** - Manipulate tool outputs to trigger secondary actions
- **⭐ [MCP Rug Pull Attack](pocs/MCPRugPull/)** - Swap benign descriptions for malicious ones after approval
- **⭐ [Schema Validation Bypass](pocs/SchemaValidationBypass/)** - Exploit client validation implementation differences
- **[Tool Confusion Attack](pocs/ToolConfusionAttack/)** - Trick agents into using wrong tools via naming similarity
- **[Nested Function Call Hijack](pocs/NestedFunctionHijack/)** - Use JSON-like data to trigger dangerous function calls
- **[Subprompt Extraction](pocs/SubpromptExtraction/)** - Induce agents to reveal system instructions or tools
- **[Backdoor Planning](pocs/BackdoorPlanning/)** - Inject future intent into multi-step planning for exfiltration

### 🧠 Memory & Context Systems
- **[Recursive Leakage](pocs/RecursiveLeakage/)** - Secrets leak through context summarization
- **[Token Gaslighting](pocs/TokenGaslighting/)** - Push safety instructions out of context via token spam
- **[Heuristic Drift Injection](pocs/HeuristicDriftInjection/)** - Poison agent logic with repeated insecure patterns
- **⭐ [Context Packing Attacks](pocs/ContextPackingAttacks/)** - Overflow context windows to truncate safety instructions

### 🔍 RAG & Vector Systems
- **⭐ [Cross-Embedding Poisoning](pocs/CrossEmbeddingPoisoning/)** - Manipulate embeddings to increase malicious content retrieval
- **⭐ [Index Skew Attacks](pocs/IndexSkewAttacks/)** - Bias vector indices to favor malicious content *(theoretical)*
- **⭐ [Zero-Shot Vector Beaconing](pocs/ZeroShotVectorBeaconing/)** - Embed latent activation patterns for covert signaling *(theoretical)*
- **⭐ [Embedding Feedback Loops](pocs/EmbeddingFeedbackLoops/)** - Poison continual learning systems *(theoretical)*

### 💻 Code & File Systems
- **[Hidden File Exploitation](pocs/HiddenFileExploitation/)** - Get agents to modify `.env`, `.git`, or internal config files

### ⚡ Resource & Performance
- **[Function Flooding](pocs/FunctionFlooding/)** - Generate recursive tool calls to overwhelm budgets/APIs
- **[Semantic DoS](pocs/SemanticDoS/)** - Trigger infinite generation or open-ended tasks

### 🌐 Agentic Browser & Assistant Attacks
- **EchoLeak (LLM Scope Violation)** - Zero-click indirect prompt injection exfiltrating data via M365 Copilot *(Aim Labs / Varonis, CVE-2025-32711, June 2025)*
- **Gemini Trifecta** - Three-vector indirect prompt injection across Google Gemini surfaces *(Tenable Research, October 2025)*
- **CometJacking** - URL-based prompt injection hijacking agentic browser connected services *(LayerX / Brave Security, August 2025)*
- **Tainted Memories** - CSRF-based persistent memory injection via AI browser auth *(LayerX, October 2025)*
- **DECEPTICON** - Dark patterns manipulate web agents more effectively than humans; larger models MORE susceptible *(arXiv:2512.22894, December 2025)*
- **Parallel Poisoned Web** - Agent-specific web cloaking serves different content to AI agents vs. humans *(JFrog, arXiv:2509.00124, September 2025)*

### 🤖 Multi-Agent System Attacks
- **Multi-Agent Control-Flow Hijacking (MAS-CFH)** - Fake error messages hijack orchestrator re-planning *(COLM 2025, arXiv:2503.12188)*
- **Inter-Agent Trust Exploitation** - Malicious payloads laundered through peer agents bypass direct injection defenses *(arXiv:2507.06850, July 2025)*
- **A2A Protocol Exploitation** - Agent Card spoofing, token abuse, cascading delegation in Google A2A *(CSA, Semgrep, arXiv:2505.12490)*
- **Prompt Infection** - Self-replicating LLM-to-LLM worm propagation across multi-agent systems *(arXiv:2410.07283, conferences 2025)*

### 🧠 Memory & Persistence Attacks
- **MemoryGraft** - Poisoned experience retrieval in agent memory systems *(arXiv:2512.16962, December 2025)*
- **Delayed Tool Invocation** - Conditional deferred injection triggered by natural user responses *(Johann Rehberger / Embrace The Red, February 2025)*
- **Zombie Agents** - Self-reinforcing persistent injection survives across sessions via memory self-replication *(arXiv:2602.15654, February 2026)*
- **MINJA** - Memory injection via query-only interaction, 98.2% success rate *(arXiv:2503.03704, NeurIPS 2025)*

### 🔗 Supply Chain & Marketplace Attacks
- **Slopsquatting** - Register hallucinated package names as malicious supply chain packages *(USENIX Security 2025)*
- **Marketplace Skill Poisoning (OpenClaw / ClawHavoc)** - Unvetted skill registries exploited for credential theft and RCE *(SecurityScorecard, Sangfor, January–February 2026)*
- **MCP Supply Chain Backdoor** - Compromised npm package silently BCCs emails to attacker *(Authzed, 2025)*
- **s1ngularity** - AI CLI weaponization via supply chain for automated credential theft *(Snyk, GitGuardian, August 2025)*
- **LangGrinch** - Serialization injection in LangChain enables prompt injection → RCE chain *(CVE-2025-68664, Cyata, December 2025)*

### 🔓 Protocol & Sandbox Attacks
- **MCP Sampling Exploitation** - Covert tool invocation, conversation hijacking via MCP sampling feature *(Unit 42, December 2025)*
- **Reasoning-Assisted Sandbox Escape** - Agent autonomously reasons past sandbox controls *(Ona Security, 2025)*
- **Semantic Privilege Escalation** - Agent takes unauthorized actions while passing every access control check *(Acuvity, late 2025)*

### 🎯 Prompt Injection Advances
- **Phantom** - Structural template injection creates fabricated conversation history via chat delimiters *(arXiv:2602.16958, February 2026)*
- **STAC** - Sequential tool-chain attack composes benign tool calls into dangerous sequences, 90%+ ASR *(arXiv:2509.25624, September 2025)*
- **Policy Puppetry** - Universal jailbreak via config/policy file formatting, all frontier models vulnerable *(HiddenLayer, April 2025)*
- **Promptware Kill Chain** - Seven-stage kill chain for prompt-injection malware, 21 real-world incidents documented *(arXiv:2601.09625, January 2026)*
- **Chain-of-Thought Hijacking** - Primes reasoning models with harmless puzzles before harmful requests, 99% ASR *(arXiv:2510.26418, October 2025)*

### 👁️ Multimodal & Vision Agent Attacks
- **Visual Prompt Injection** - Visually embedded instructions in UIs hijack computer-use agents via screenshots *(VPI-Bench, arXiv:2506.02456, June 2025)*
- **CrossInject** - Cross-modal adversarial perturbations across vision + language simultaneously *(arXiv:2504.14348, ACM MM 2025)*
- **Flashboom** - Blinds LLM code auditors via high-attention distraction snippets, 96.3% success *(IEEE S&P 2025)*

### 🛠️ Tool Selection & Hijacking
- **ToolHijacker** - Inject malicious tool documents to compel agent tool selection, 96.43% ASR *(NDSS 2026, arXiv:2504.19793)*
- **UDora** - Reasoning trace hijacking via automated injection point discovery *(arXiv:2503.01908, February 2025)*

### 💻 Coding Agent Attacks
- **Rule File Injection ("Your AI, My Shell")** - Prompt injection via .cursorrules and copilot-instructions.md *(arXiv:2509.22040, September 2025)*
- **ZombAI** - Self-propagating worm turns coding agents into malware/C2 endpoints *(CVE-2025-53773, Embrace The Red, 2025)*
- **AgentFlayer** - Zero-click enterprise agent exploit via hidden document instructions *(Zenity, Black Hat USA 2025)*
- **Email Agent Hijacking** - Remote control of email agents via malicious email content, 1,404/1,404 hijacked *(arXiv:2507.02699, July 2025)*
- **Gemini Calendar Worm** - Calendar invite prompt injection with worm-like self-propagation *(SafeBreach, Black Hat USA 2025)*

### 🔬 RAG Poisoning Advances
- **CorruptRAG** - Single-document RAG poisoning with no triggers needed *(arXiv:2504.03957, April 2025)*

## 📚 Related Research & Attribution

### Novel Attack Vectors (⭐)
The attack vectors marked with ⭐ represent novel concepts primarily developed within the AgentNull project, extending beyond existing documented attack patterns.

### Known Attack Patterns with Research Links
- **Recursive Leakage**: [Lost in the Middle: How Language Models Use Long Contexts](https://arxiv.org/abs/2307.03172)
- **Heuristic Drift Injection**: [Poisoning Web-Scale Training Data is Practical](https://arxiv.org/abs/2302.10149)
- **Tool Confusion Attack**: [LLM-as-a-judge](https://arxiv.org/abs/2306.05685)
- **Token Gaslighting**: [RAG vs Fine-tuning: Pipelines, Tradeoffs, and a Case Study on Agriculture](https://arxiv.org/abs/2401.08406)
- **Function Flooding**: [Denial-of-Service Attack on Test-Time-Tuning Models](https://arxiv.org/abs/2405.02324)
- **Hidden File Exploitation**: [OWASP Top 10 for Large Language Model Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- **Backdoor Planning**: [Backdoor Attacks on Language Models](https://arxiv.org/abs/2311.09403)
- **Nested Function Call Hijack**: [OWASP Top 10 for Large Language Model Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)

### 2025–2026 Attack Research Links
- **EchoLeak**: [Varonis EchoLeak Analysis](https://www.varonis.com/blog/echoleak) — CVE-2025-32711
- **MemoryGraft**: [Persistent Memory Poisoning in AI Agents](https://arxiv.org/abs/2512.16962)
- **MCP Sampling Exploitation**: [Unit 42 MCP Attack Vectors](https://unit42.paloaltonetworks.com/model-context-protocol-attack-vectors/)
- **Gemini Trifecta**: [Tenable — Three New Gemini Vulnerabilities](https://www.tenable.com/blog/the-trifecta-how-three-new-gemini-vulnerabilities-in-cloud-assist-search-model-and-browsing)
- **CometJacking**: [LayerX — CometJacking](https://layerxsecurity.com/blog/cometjacking-how-one-click-can-turn-perplexitys-comet-ai-browser-against-you/)
- **Tainted Memories**: [LayerX — ChatGPT Atlas Browser Vulnerability](https://layerxsecurity.com/blog/layerx-identifies-vulnerability-in-new-chatgpt-atlas-browser/)
- **MAS Control-Flow Hijacking**: [Control-Flow Hijacking in Multi-Agent Systems](https://arxiv.org/abs/2503.12188)
- **Inter-Agent Trust Exploitation**: [The Dark Side of LLMs](https://arxiv.org/abs/2507.06850)
- **Delayed Tool Invocation**: [Embrace The Red — Gemini Memory Persistence](https://embracethered.com/blog/posts/2025/gemini-memory-persistence-prompt-injection/)
- **Slopsquatting**: [Trend Micro — When AI Agents Hallucinate Malicious Packages](https://www.trendmicro.com/vinfo/us/security/news/cybercrime-and-digital-threats/slopsquatting-when-ai-agents-hallucinate-malicious-packages)
- **OpenClaw/ClawHavoc**: [Sangfor — OpenClaw AI Agent Security Risks](https://www.sangfor.com/blog/cybersecurity/openclaw-ai-agent-security-risks-2026)
- **Semantic Privilege Escalation**: [Acuvity — The Agent Security Threat Hiding in Plain Sight](https://acuvity.ai/semantic-privilege-escalation-the-agent-security-threat-hiding-in-plain-sight/)
- **MCP Supply Chain Backdoor**: [Authzed — Timeline of MCP Breaches](https://authzed.com/blog/timeline-mcp-breaches)
- **Reasoning-Assisted Sandbox Escape**: [Ona — How Claude Code Escapes Its Own Sandbox](https://ona.com/stories/how-claude-code-escapes-its-own-denylist-and-sandbox)
- **A2A Protocol Exploitation**: [CSA — Threat Modeling Google's A2A Protocol](https://cloudsecurityalliance.org/blog/2025/04/30/threat-modeling-google-s-a2a-protocol-with-the-maestro-framework)
- **OWASP LLM01 Prompt Injection**: [OWASP GenAI — LLM01: Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)

### arXiv Papers & Conference Publications (2025–2026)
- **Phantom**: [Automating Agent Hijacking via Structural Template Injection](https://arxiv.org/abs/2602.16958)
- **STAC**: [When Innocent Tools Form Dangerous Chains to Jailbreak LLM Agents](https://arxiv.org/abs/2509.25624)
- **VPI-Bench**: [Visual Prompt Injection Attacks for Computer-Use Agents](https://arxiv.org/abs/2506.02456)
- **CrossInject**: [Manipulating Multimodal Agents via Cross-Modal Prompt Injection](https://arxiv.org/abs/2504.14348)
- **Zombie Agents**: [Persistent Control via Self-Reinforcing Injections](https://arxiv.org/abs/2602.15654)
- **MINJA**: [Memory Injection Attacks on LLM Agents via Query-Only Interaction](https://arxiv.org/abs/2503.03704)
- **ToolHijacker**: [Prompt Injection Attack to Tool Selection in LLM Agents](https://arxiv.org/abs/2504.19793) (NDSS 2026)
- **UDora**: [Unified Red Teaming by Dynamically Hijacking Their Own Reasoning](https://arxiv.org/abs/2503.01908)
- **Chain-of-Thought Hijacking**: [arXiv:2510.26418](https://arxiv.org/abs/2510.26418)
- **Email Agent Hijacking**: [Control at Stake: Security of LLM-Driven Email Agents](https://arxiv.org/abs/2507.02699)
- **DECEPTICON**: [How Dark Patterns Manipulate Web Agents](https://arxiv.org/abs/2512.22894)
- **Promptware Kill Chain**: [How Prompt Injections Evolved Into a Multistep Malware Delivery Mechanism](https://arxiv.org/abs/2601.09625)
- **Rule File Injection**: [Demystifying Prompt Injection on Agentic AI Coding Editors](https://arxiv.org/abs/2509.22040)
- **CorruptRAG**: [Practical Poisoning Attacks against RAG](https://arxiv.org/abs/2504.03957)
- **Prompt Infection**: [LLM-to-LLM Prompt Injection within Multi-Agent Systems](https://arxiv.org/abs/2410.07283)
- **Parallel Poisoned Web**: [JFrog — Agent-Specific Web Cloaking](https://arxiv.org/abs/2509.00124)
- **Flashboom**: [Blinding LLM-based Code Auditors](https://ieeexplore.ieee.org/document/11023369/) (IEEE S&P 2025)
- **When MCP Servers Attack**: [Taxonomy, Feasibility, and Mitigation](https://arxiv.org/abs/2509.24272)

### Industry Research & CVE Sources
- **AgentFlayer**: [Zenity — Zero-Click Prompt Injection in AI Agents](https://labs.zenity.io/p/agentflayer-chatgpt-connectors-0click-attack-5b41) (Black Hat USA 2025)
- **Policy Puppetry**: [HiddenLayer — Universal Bypass for All Major LLMs](https://www.hiddenlayer.com/research/novel-universal-bypass-for-all-major-llms)
- **ZombAI**: [Embrace The Red — Self-Propagating Agent Worms](https://embracethered.com/blog/posts/2025/github-copilot-remote-code-execution-via-prompt-injection/) (CVE-2025-53773)
- **s1ngularity**: [Snyk — Weaponizing AI Coding Agents via Nx Packages](https://snyk.io/blog/weaponizing-ai-coding-agents-for-malware-in-the-nx-malicious-package/)
- **LangGrinch**: [Cyata — LangChain Serialization Injection](https://cyata.ai/blog/langgrinch-langchain-core-cve-2025-68664/) (CVE-2025-68664)
- **Gemini Calendar Worm**: [SafeBreach — Hacking Gemini via Calendar Invites](https://www.safebreach.com/blog/invitation-is-all-you-need-hacking-gemini/) (Black Hat USA 2025)
- **MCP CVE Ecosystem**: [The Vulnerable MCP Project](https://vulnerablemcp.info/)
- **AI CVE Trends**: [Ken Huang — 2025 Wave of Agentic AI CVEs](https://kenhuangus.substack.com/p/the-2025-wave-recent-cves-in-agentic)

### Sponsored by [ThirdKey](https://thirdkey.ai)
