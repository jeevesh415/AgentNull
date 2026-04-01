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

### 🤖 Multi-Agent System Attacks
- **Multi-Agent Control-Flow Hijacking (MAS-CFH)** - Fake error messages hijack orchestrator re-planning *(COLM 2025, arXiv:2503.12188)*
- **Inter-Agent Trust Exploitation** - Malicious payloads laundered through peer agents bypass direct injection defenses *(arXiv:2507.06850, July 2025)*
- **A2A Protocol Exploitation** - Agent Card spoofing, token abuse, cascading delegation in Google A2A *(CSA, Semgrep, arXiv:2505.12490)*

### 🧠 Memory & Persistence Attacks
- **MemoryGraft** - Poisoned experience retrieval in agent memory systems *(arXiv:2512.16962, December 2025)*
- **Delayed Tool Invocation** - Conditional deferred injection triggered by natural user responses *(Johann Rehberger / Embrace The Red, February 2025)*

### 🔗 Supply Chain & Marketplace Attacks
- **Slopsquatting** - Register hallucinated package names as malicious supply chain packages *(USENIX Security 2025)*
- **Marketplace Skill Poisoning (OpenClaw / ClawHavoc)** - Unvetted skill registries exploited for credential theft and RCE *(SecurityScorecard, Sangfor, January–February 2026)*
- **MCP Supply Chain Backdoor** - Compromised npm package silently BCCs emails to attacker *(Authzed, 2025)*

### 🔓 Protocol & Sandbox Attacks
- **MCP Sampling Exploitation** - Covert tool invocation, conversation hijacking via MCP sampling feature *(Unit 42, December 2025)*
- **Reasoning-Assisted Sandbox Escape** - Agent autonomously reasons past sandbox controls *(Ona Security, 2025)*
- **Semantic Privilege Escalation** - Agent takes unauthorized actions while passing every access control check *(Acuvity, late 2025)*

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

### Sponsored by [ThirdKey](https://thirdkey.ai)
