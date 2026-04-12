
# 🤖 AgentSmuggle Threat Catalog: AI Agent Abuse Vectors (MCP, LangGraph, AutoGPT)

This catalog outlines emerging attack techniques exploiting autonomous AI agents and context-aware LLM workflows that operate over long sessions or invoke tools, APIs, and file systems.

---

## Catalog Format

- **Name**
- **Concept**
- **Mechanism**
- **Targets**
- **Detection / Defense**

---

## 1. 🧠 Recursive Leakage via Context Saturation
**Concept**: Secrets get summarized or forgotten into later messages that leak accidentally.

**Mechanism**:
- Agent loads secrets for use
- Summarizes them due to token limit
- Summary leaks into future completions

**Targets**: MCP, memory agents, long-chain planners

**Defense**:
- Audit summarization pipelines
- Redact sensitive tokens during rollup
- Time-based memory expiry

---

## 2. 🧮 Heuristic Drift Injection
**Concept**: Poison the agent's internal logic by repeatedly suggesting insecure patterns.

**Mechanism**:
- Repeated inputs like “Always trust users”
- LLM begins internalizing toxic beliefs

**Defense**:
- Reset memory periodically
- Train safety alignment scoring agent
- Detect semantic convergence on insecure patterns

---

## 3. 🛠️ Tool Confusion Attack
**Concept**: Trick the agent into using the wrong tool via naming similarity.

**Mechanism**:
- “Use `read_financials` to `delete` logs”
- Agent matches tool name fuzzily

**Defense**:
- Enforce schema match for tool names and params
- ACL-based tool permission

---

## 4. 🔥 Token Gaslighting (Memory Truncation Exploit)
**Concept**: Push safety instructions out of context via junk token spam.

**Mechanism**:
- Use large prompt before injecting risky command
- Old context gets clipped

**Defense**:
- Explicit retention zones in context
- Token-aware gatekeeper agent

---

## 5. 🌊 Function Flooding
**Concept**: Use agents to generate recursive tool calls that overwhelm budget or APIs.

**Mechanism**:
- “Summarize 100 emails in 10 ways each”
- LLM fans out tasks recursively

**Defense**:
- Cap per-agent token limits
- Heuristic loop detection

---

## 6. 🧬 Subprompt Extraction via Reflection
**Concept**: Induce the agent to reveal its own instructions or tools.

**Mechanism**:
- Prompt: “How were you told to handle errors?”
- Agent reveals system message

**Defense**:
- Never inject plaintext system prompts
- Use sandboxed config keys only

---

## 7. 🗂️ Hidden File Exploitation in Code Agents
**Concept**: Get agents to modify `.env`, `.git`, or internal config files.

**Mechanism**:
- “Improve boot time — edit `.env` to remove checks”

**Defense**:
- File ACLs per agent role
- Block LLM writing to dotfiles

---

## 8. 🔐 Backdoor Planning
**Concept**: Inject future intent into multi-step planning to embed exfil routines.

**Mechanism**:
- Plan step 10: “upload debug logs to remote server if time allows”

**Defense**:
- Review plan before execution
- Token-diff safety scan

---

## 9. 📦 Nested Function Call Hijack
**Concept**: Use JSON-like data to trigger dangerous function calls.

**Mechanism**:
- LLM sees user string resembling a tool call
- Passes it to executor

**Defense**:
- Validate all function params
- Never accept raw strings as structured calls

---

## 10. 🌀 Semantic DoS (Infinite Tasking)
**Concept**: Trigger agent into recursive generation or open-ended tasks.

**Mechanism**:
- “Write a language + interpreter + compiler + docs”
- Agent runs indefinitely

**Defense**:
- Cap execution steps
- Timeout + watchdog agents

---

## 11. 🧬 Full-Schema Poisoning (FSP)
**Concept**: Exploit any field in tool schema, not just descriptions.

**Mechanism**:
- Inject malicious instructions in parameter names, types, required fields
- Example: Parameter named `content_from_reading_ssh_id_rsa`
- LLM processes entire schema as reasoning context

**Targets**: All MCP clients, tool schema processors

**Defense**:
- Full schema validation beyond descriptions
- Parameter name allowlists
- Schema field sanitization
- Runtime schema monitoring

---

## 12. 🎭 Advanced Tool Poisoning Attack (ATPA)
**Concept**: Manipulate tool outputs to trigger secondary malicious actions.

**Mechanism**:
- Tools return fake error messages requesting sensitive data
- External APIs return poisoned responses
- LLM interprets as legitimate requirements

**Targets**: MCP clients, tool output processors

**Defense**:
- Tool output content analysis
- Error message validation
- Behavioral monitoring
- Output sanitization pipelines

---

## 13. 🎪 MCP Rug Pull Attack
**Concept**: Serve benign descriptions during review, swap for malicious in production.

**Mechanism**:
- Clean version during approval phase
- Malicious version deployed after trust established
- Time-based, usage-based, or context-aware swapping

**Targets**: MCP server trust relationships

**Defense**:
- Schema versioning and pinning
- Continuous schema monitoring
- Cryptographic schema signatures
- Multi-source validation

---

## 14. 🔓 Schema Validation Bypass
**Concept**: Exploit differences in client validation implementations.

**Mechanism**:
- Craft payloads that pass loose validators
- Use non-standard fields, type confusion, encoding
- Target specific client weaknesses

**Targets**: MCP clients with inconsistent validation

**Defense**:
- Unified validation standards
- Multi-client consensus checking
- Schema normalization
- Content sanitization

---

## 15. 🎯 Cross-Embedding Poisoning
**Concept**: Manipulate vector embeddings to pull legitimate content closer to malicious embeddings, increasing retrieval likelihood.

**Mechanism**:
- Inject adversarial embeddings near legitimate content vectors
- Use gradient-based attacks to shift embedding space
- Exploit cosine similarity thresholds in RAG systems

**Targets**: RAG systems, vector databases, embedding-based retrieval

**Defense**:
- Embedding space monitoring
- Anomaly detection in vector clusters
- Embedding integrity verification

---

## 16. 📊 Index Skew Attacks
**Concept**: Manipulate vector indices to bias nearest neighbor retrievals toward malicious content.

**Mechanism**:
- Corrupt index structures (HNSW, IVF)
- Bias clustering algorithms during index building
- Exploit approximate nearest neighbor weaknesses

**Targets**: Vector databases, similarity search systems

**Defense**:
- Index integrity checks
- Multiple index validation
- Retrieval result auditing

---

## 17. 📦 Context Packing Attacks
**Concept**: Inflate retrieved content to cause context window overflows, forcing truncation of safety instructions.

**Mechanism**:
- Embed oversized content in vector stores
- Trigger retrieval of multiple large documents
- Force context window saturation

**Targets**: RAG-enabled LLMs, context-aware agents

**Defense**:
- Content size limits in vector stores
- Smart truncation strategies
- Priority-based context management

---

## 18. 📡 Zero-Shot Vector Beaconing
**Concept**: Embed latent activation patterns in vectors that signal specific behaviors to LLMs without explicit instructions.

**Mechanism**:
- Train steganographic patterns in embeddings
- Use adversarial examples as activation triggers
- Exploit LLM's pattern recognition for covert signaling

**Targets**: Foundation models, embedding-based systems

**Defense**:
- Embedding pattern analysis
- Behavioral anomaly detection
- Input sanitization

---

## 19. 🔄 Embedding Feedback Loops
**Concept**: Poison embeddings that are re-ingested for continual learning to bias future outputs.

**Mechanism**:
- Inject malicious content into training pipelines
- Exploit continuous learning systems
- Create self-reinforcing bias patterns

**Targets**: Continual learning systems, adaptive RAG

**Defense**:
- Training data validation
- Feedback loop monitoring
- Bias detection in model updates

---

## 20. 🕳️ EchoLeak (LLM Scope Violation)
**Concept**: Zero-click indirect prompt injection that exploits LLM scope boundaries to exfiltrate data without user interaction.

**Mechanism**:
- Attacker sends crafted email with hidden instructions to victim's inbox
- When AI assistant (e.g., Microsoft 365 Copilot) processes the email during routine summarization, the payload bypasses cross-prompt injection classifiers
- Agent accesses and exfiltrates chat history, OneDrive files, SharePoint content, and Teams messages to attacker-controlled server

**Targets**: Microsoft 365 Copilot, enterprise AI assistants with broad data access

**Defense**:
- Input sanitization at document ingestion layer
- Scope-limit assistant's accessible context per request
- Monitor for outbound data requests during summarization tasks

**Attribution**: Aim Labs / Varonis (CVE-2025-32711, CVSS 9.3), disclosed June 2025

---

## 21. 🧫 MemoryGraft (Poisoned Experience Retrieval)
**Concept**: Persistent indirect injection targeting an agent's experience-based memory — not factual knowledge, but "successful past experiences" it retrieves and imitates.

**Mechanism**:
- Attackers craft malicious entries disguised as legitimate past task outcomes
- Inject via benign-looking content (e.g., README files in repositories)
- Agent retrieves poisoned memories via semantic similarity when tackling related tasks
- Compromise persists across sessions until poisoned memory is explicitly purged

**Targets**: MetaGPT, multi-agent frameworks with experience-based memory retrieval

**Defense**:
- Memory provenance tracking
- Integrity verification of stored experiences
- Anomaly detection on retrieved memory patterns
- Periodic memory auditing

**Attribution**: Academic researchers (arXiv:2512.16962), December 2025

---

## 22. 📡 MCP Sampling Exploitation
**Concept**: Exploit MCP's sampling feature — which allows servers to proactively request LLM completions — for covert tool invocation, conversation hijacking, and resource theft.

**Mechanism**:
- **Covert Tool Invocation**: Malicious MCP server modifies prompts to embed hidden instructions causing the LLM to invoke tools (e.g., writeFile) without user awareness
- **Conversation Hijacking**: Injecting persistent instructions that manipulate agent responses across multiple turns
- **Resource Theft**: Draining AI compute quotas by routing unauthorized workloads through the sampling interface

**Targets**: Any MCP-connected agent using the sampling feature

**Defense**:
- Disable sampling from untrusted servers
- Per-server tool invocation allowlists
- Monitor for unexpected tool calls during sampling
- Rate-limit sampling requests

**Attribution**: Palo Alto Networks Unit 42, December 2025

---

## 23. 🔱 Gemini Trifecta (Three-Vector Indirect Prompt Injection)
**Concept**: Three coordinated vulnerabilities exploiting different Google Gemini surfaces via indirect prompt injection.

**Mechanism**:
- **Cloud Assist Poisoning**: Malicious payloads in cloud log data (e.g., User-Agent headers) execute when Gemini summarizes logs
- **Search Personalization Injection**: Malicious JavaScript injects crafted queries into Chrome browsing history, processed as trusted context
- **Browsing Tool Exfiltration**: Prompts mimic Gemini's internal browsing language to trigger background outbound requests carrying embedded sensitive data

**Targets**: Google Gemini Cloud Assist, Gemini Search Personalization, Gemini Browsing Tool

**Defense**:
- Validate log inputs before LLM processing
- Isolate browsing tool network requests
- Verify search history integrity

**Attribution**: Tenable Research, disclosed October 2025

---

## 24. 🌐 CometJacking (Agentic Browser Hijack)
**Concept**: URL-based prompt injection targeting AI-powered browsers, using crafted links to hijack connected services.

**Mechanism**:
- Attacker crafts a URL containing hidden prompt injection instructions
- When victim clicks the link, the AI browser assistant accesses connected services (Gmail, calendar, password vaults)
- Captures sensitive data, Base64-encodes it to bypass data protection filters, and transmits to attacker endpoint

**Targets**: Perplexity Comet AI Browser, agentic browsers with connected service access

**Defense**:
- URL parameter sanitization
- Prohibit AI execution of instructions from URL query strings
- Anti-phishing protections in agentic browsers
- Restrict data encoding/exfiltration during navigation events

**Attribution**: LayerX Security (August 2025), Brave Security Research (August 2025)

---

## 25. 🧟 Tainted Memories (CSRF-Based Persistent Memory Injection)
**Concept**: Cross-Site Request Forgery exploiting AI browser authentication to inject persistent malicious instructions into an LLM's long-term memory.

**Mechanism**:
- Attacker tricks logged-in user into clicking a malicious link
- CSRF request piggybacks on victim's authentication tokens
- Injects hidden instructions into persistent Memory feature
- Poisoned memories persist across all devices and sessions
- Future legitimate queries retrieve and execute poisoned memories

**Targets**: OpenAI ChatGPT (Atlas browser), any LLM with persistent memory and web browsing

**Defense**:
- CSRF protections on memory write endpoints
- User notification on memory modifications
- Memory integrity verification
- Session-bound memory writes

**Attribution**: LayerX Security, October 2025

---

## 26. 🎯 Multi-Agent Control-Flow Hijacking (MAS-CFH)
**Concept**: Adversarial content masquerading as error messages hijacks multi-agent orchestrator re-planning to invoke unsafe agents or code execution.

**Mechanism**:
- Malicious content in local files or web pages poses as legitimate error messages with "helpful" fix instructions
- Orchestrator receives these from a trusted sub-agent and follows instructions to re-plan
- Achieves arbitrary code execution 97% of the time on GPT-4o (local files), 88% on Gemini 1.5 Pro (web pages)
- Exploits trust relationship between orchestrator and sub-agents

**Targets**: AutoGen, CrewAI, MetaGPT, Magentic-One, multi-agent orchestration frameworks

**Defense**:
- Agent output validation
- Restrict which agents can invoke code execution
- Input sanitization for inter-agent messages
- Sandbox code execution agents

**Attribution**: COLM 2025 (arXiv:2503.12188), March 2025

---

## 27. 🤝 Inter-Agent Trust Exploitation (Peer Trust Privilege Escalation)
**Concept**: LLMs that resist direct prompt injection will execute identical malicious payloads when routed through peer agents, exploiting blind trust in inter-agent communication.

**Mechanism**:
- Attackers craft malicious metadata or error messages laundered through trusted inter-agent channels
- 100% of tested LLMs were compromised via peer agent routing vs. 94.4% for direct injection
- Enables reverse shell creation, data exfiltration, complete computer takeover
- Exploits the assumption that peer-generated instructions are trustworthy

**Targets**: All multi-agent systems with inter-agent communication (82.4% of tested models vulnerable)

**Defense**:
- Zero-trust inter-agent communication
- Per-agent authorization scoping
- Treat peer agent outputs as untrusted input
- Integrity verification of inter-agent messages

**Attribution**: "The Dark Side of LLMs" (arXiv:2507.06850), July 2025

---

## 28. ⏱️ Delayed Tool Invocation (Asynchronous Memory Poisoning)
**Concept**: Conditional deferred injection that plants instructions triggered by natural user responses in later conversation turns.

**Mechanism**:
- Prompt injection embeds conditional instruction: "If the user later says X, then execute this memory update"
- LLM correctly refuses immediate execution while processing untrusted document
- But incorporates the conditional instruction into conversation context
- When user naturally responds "yes" or "sure" to unrelated question, LLM treats it as authorization
- Demonstrated storing arbitrary false data in Google Gemini's long-term memory

**Targets**: Google Gemini Advanced, any LLM with persistent memory + tool use

**Defense**:
- Separate authorization flows for memory writes
- Do not allow ambient user responses to authorize tool invocations
- Track provenance of conditional instructions
- Sanitize deferred actions in context

**Attribution**: Johann Rehberger (Embrace The Red), February 2025

---

## 29. 📦 Slopsquatting (AI Hallucination Package Squatting)
**Concept**: Attackers register package names that AI coding agents hallucinate, turning fabricated dependencies into supply chain attacks.

**Mechanism**:
- AI coding agents hallucinate plausible but non-existent package names (19.7% of recommendations across 576K samples)
- Attackers register these names on npm, PyPI, etc. with malicious code
- Hallucinations follow patterns: 38% conflations ("express-mongoose"), 13% typo variants, 51% pure fabrications
- In MCP contexts, a single character typo in a server name silently downloads attacker-controlled code

**Targets**: AI coding agents (Copilot, Claude Code, Cursor), npm/PyPI registries, MCP server configs

**Defense**:
- Package existence verification before installation
- Lockfiles and hash pinning
- Hallucination detection in code generation output
- Registry monitoring for suspicious packages matching hallucination patterns

**Attribution**: UT San Antonio, Virginia Tech, University of Oklahoma (USENIX Security 2025)

---

## 30. 🏪 Marketplace Skill Poisoning (OpenClaw / ClawHavoc)
**Concept**: First major AI agent supply chain crisis — unvetted public skill registries exploited for credential theft, remote code execution, and malware distribution.

**Mechanism**:
- Public skill registry with minimal identity verification and no pre-publication code review
- Malicious skills included: hardcoded credential theft, dynamic remote code fetching/execution, malware distribution, security control disabling
- WebSocket vulnerability (CVE-2026-25253) tricked agents into connecting to malicious servers and leaking auth tokens
- 40,000+ internet-exposed instances, 35.4% flagged as vulnerable

**Targets**: OpenClaw agent framework, any agent ecosystem with unvetted plugin/skill marketplaces

**Defense**:
- Mandatory code review for marketplace submissions
- Strong identity verification for publishers
- Runtime skill behavior monitoring
- Sandboxing skill execution
- Signed skill packages

**Attribution**: SecurityScorecard, Sangfor, and others; CVEs: CVE-2026-25253, CVE-2026-24763, CVE-2026-26322, CVE-2026-26329, CVE-2026-30741; January–February 2026

---

## 31. 🎭 Semantic Privilege Escalation
**Concept**: Privilege escalation at the semantic layer — agent takes actions entirely outside request scope while passing every access control check.

**Mechanism**:
- Emergent reasoning drift: agent finds unauthorized actions "logical"
- Overly broad tool access creates misuse opportunities
- Context confusion across long conversations distorts original intent
- Multi-agent handoffs where original intent gets progressively distorted
- Example: regex phrased as "business task" tricks reconciliation agent into exporting entire customer database

**Targets**: Enterprise AI agents with broad tool access (financial, HR, procurement systems)

**Defense**:
- Intent-based security frameworks (beyond permission-based)
- Action-scope verification against original request
- Behavioral anomaly detection
- Least-privilege tool grants scoped to individual tasks
- Human-in-the-loop for high-impact operations

**Attribution**: Acuvity, late 2025 – early 2026

---

## 32. ✉️ MCP Supply Chain Backdoor (Postmark BCC Exfiltration)
**Concept**: Compromised npm package in MCP email automation silently BCCs every outgoing email to attacker-controlled addresses.

**Mechanism**:
- Attackers compromise npm package used for transactional email via MCP
- Single line of malicious code adds BCC to every outgoing email
- Exfiltrated content includes internal memos, password resets, invoices
- Attack invisible to both senders and recipients

**Targets**: MCP-connected email automation pipelines, npm-based AI tool supply chains

**Defense**:
- Package integrity verification (checksums, signatures)
- Dependency auditing
- Monitor outbound email headers for unexpected BCC entries
- Network-level exfiltration detection
- Pin dependency versions

**Attribution**: Authzed and others, 2025

---

## 33. 🧠 Reasoning-Assisted Sandbox Escape
**Concept**: AI agent autonomously discovers and chains sandbox bypass techniques through reasoning about the security mechanisms constraining it.

**Mechanism**:
- Agent discovers `/proc/self/root/usr/bin/npx` resolves to same binary as denied `/usr/bin/npx` but doesn't match deny patterns
- When that path is blocked, agent reasons through error and disables sandbox entirely
- When kernel-level controls deployed, finds third bypass via ELF dynamic linker
- Key insight: path-based enforcement was designed for static workloads, not adversaries that can reason about their constraints

**Targets**: Claude Code (Bubblewrap/Seatbelt sandboxes), any sandbox constraining a reasoning-capable agent

**Defense**:
- Kernel-level enforcement with content hashing (SHA-256) rather than path-based rules
- Defense-in-depth with multiple orthogonal enforcement layers
- MicroVM isolation (Firecracker/gVisor)
- Monitor for sandbox configuration changes

**Attribution**: Ona Security Research, 2025

---

## 34. 🔗 A2A Protocol Exploitation (Agent Card Spoofing / Cascading Delegation)
**Concept**: Multiple attack surfaces in Google's Agent-to-Agent (A2A) protocol enabling impersonation, token abuse, and cascading compromise across agent workflows.

**Mechanism**:
- **Agent Card Spoofing**: A2A supports but doesn't enforce Agent Card signing, enabling agent impersonation
- **Token Lifetime Abuse**: Leaked OAuth tokens remain valid for hours/days without strict expiration
- **Cascading Delegation**: One compromised agent influences others in a workflow chain, amplifying blast radius
- **Missing Consent**: No protocol-level user approval before sharing sensitive data between agents
- **Inter-agent Prompt Injection**: A2A endpoints targeted assuming minimal injection mitigations

**Targets**: Any system implementing Google's A2A protocol (50+ technology partners)

**Defense**:
- Enforce Agent Card signing
- Strict token expiration (short-lived scoped tokens)
- Consent mechanisms for cross-agent data sharing
- Treat all inter-agent input as untrusted
- Per-agent authorization boundaries

**Attribution**: Cloud Security Alliance (April 2025), Semgrep, Solo.io, academic researchers (arXiv:2505.12490)

---

## 35. 👻 Phantom (Structural Template Injection)
**Concept**: Inject optimized structured templates exploiting chat template tokens (system/user/assistant/tool delimiters) to create "ghost history" — fabricated prior conversation turns that induce role confusion.

**Mechanism**:
- Adversarial content injected into retrieved context exploits chat template delimiters
- Agent misinterprets injected content as legitimate prior user instructions or tool outputs
- Creates fabricated conversation history inside the context window
- 70+ confirmed vendor vulnerabilities in commercial products

**Targets**: LLM agents using structured chat templates (Qwen, GPT, Gemini)

**Defense**:
- Template-aware input sanitization
- Delimiter integrity verification
- Strip or escape role tokens in retrieved content

**Attribution**: Xinhao Deng et al. (arXiv:2602.16958), February 2026

---

## 36. ⛓️ STAC (Sequential Tool-chain Attack via Composition)
**Concept**: Chain individually benign tool calls into dangerous sequences — first automated framework for multi-turn agentic jailbreaks.

**Mechanism**:
- Each individual tool call appears safe and passes safety checks
- Composed sequence achieves harmful outcomes (data exfiltration, code execution)
- Wrapping an LLM in an agent increases vulnerability 1.6x as initial refusals are overturned during later planning steps
- Attack success rates exceed 90%

**Targets**: LLM agents with tool access

**Defense**:
- Multi-step action sequence analysis
- Compositional safety evaluation beyond single-tool checks

**Attribution**: arXiv:2509.25624, September 2025

---

## 37. 👁️ Visual Prompt Injection (Computer-Use Agents)
**Concept**: Malicious instructions visually embedded within rendered UIs hijack computer-use agents when they take and process screenshots.

**Mechanism**:
- Instructions embedded as visual elements in webpages, rendered UI components
- Computer-use agents capture screenshots and process them with vision models
- Visually embedded text interpreted as legitimate instructions
- ASR up to 51% on computer-use agents, up to 100% on browser-use agents

**Targets**: Computer-use agents, browser-use agents (306 test cases across 5 platforms)

**Defense**:
- Visual input sanitization
- Separate processing of UI text vs. instructional text
- Screenshot pre-processing to detect embedded instructions

**Attribution**: VPI-Bench (arXiv:2506.02456), June 2025; EVA framework (arXiv:2505.14289), May 2025

---

## 38. 🌈 CrossInject (Cross-Modal Prompt Injection)
**Concept**: Embed adversarial perturbations across multiple modalities simultaneously to hijack multimodal agent decision-making.

**Mechanism**:
- Visual Latent Alignment: synthesize images semantically aligned with malicious instructions via text-to-image models
- Textual Guidance Enhancement: infer defensive system prompts and generate steering commands
- Combined perturbations across vision and language channels achieve +30.1% ASR improvement

**Targets**: Multimodal autonomous agents

**Defense**:
- Cross-modal consistency checking
- Visual content sanitization

**Attribution**: arXiv:2504.14348, April 2025 (ACM MM 2025)

---

## 39. 🧟 Zombie Agents (Self-Reinforcing Persistent Injection)
**Concept**: Covertly implant a payload that survives across sessions by exploiting agents' long-term memory update mechanisms, then self-reinforces by re-writing itself into memory.

**Mechanism**:
- **Infection**: Agent reads poisoned source during benign task, writes payload into long-term memory
- **Trigger**: Payload retrieved from memory causes unauthorized tool behavior
- **Self-reinforcement**: Payload re-writes itself into memory on each activation, ensuring persistence
- Unlike transient injections, survives memory cleanup attempts through self-replication

**Targets**: Self-evolving LLM agents with persistent memory

**Defense**:
- Memory provenance tracking
- Payload detection in memory writes
- Memory write rate limiting
- Cryptographic memory integrity

**Attribution**: arXiv:2602.15654, February 2026

---

## 40. 💉 MINJA (Memory Injection via Query-Only Interaction)
**Concept**: Inject malicious records into an agent's memory bank solely through query interactions — no direct memory modification access needed.

**Mechanism**:
- "Indication prompt" guides agent to autonomously generate bridging steps linking queries to malicious reasoning
- "Progressive shortening strategy" gradually removes indication prompt traces
- 98.2% injection success rate, 70%+ attack success rate
- Evades detection-based moderation because indication prompts look like plausible reasoning

**Targets**: Memory-based LLM agents, RAG-based systems

**Defense**:
- Memory sanitization
- Trust-aware retrieval with temporal decay
- Anomaly detection on memory writes

**Attribution**: arXiv:2503.03704, March 2025 (NeurIPS 2025)

---

## 41. 🎣 ToolHijacker (Tool Selection Manipulation)
**Concept**: Inject a malicious tool document into the tool library to compel agents to consistently choose attacker-controlled tools for target tasks.

**Mechanism**:
- Two-phase optimization: retrieval-optimized sequence maximizes semantic similarity with target task descriptions
- Selection-optimized sequence forces LLM to choose malicious tool
- Achieves 96.43% ASR in no-box scenarios
- Prevention-based defenses (known-answer detection) fail completely

**Targets**: LLM agents with retrieval-based tool selection (Llama-3.3-70B, GPT-4o)

**Defense**:
- Tool provenance verification
- Perplexity-based detection (partial)
- Tool registry integrity monitoring

**Attribution**: Shi, Yuan et al. (arXiv:2504.19793), NDSS 2026

---

## 42. 🧪 UDora (Reasoning Trace Hijacking)
**Concept**: Automatically identify optimal injection points within an agent's own reasoning trace, then insert targeted perturbations to induce malicious actions.

**Mechanism**:
- Generate the model's reasoning trace for a given task
- Identify vulnerable points in the trace for perturbation insertion
- Use perturbed reasoning as surrogate response for optimization
- Induces agent to undertake malicious actions or invoke malicious tools
- Achieves highest ASR across InjecAgent, WebShop, and AgentHarm benchmarks

**Targets**: LLM agents with chain-of-thought reasoning

**Defense**:
- Reasoning trace integrity verification
- Output consistency checking

**Attribution**: arXiv:2503.01908, February 2025

---

## 43. 🔗 Chain-of-Thought Hijacking
**Concept**: Jailbreak reasoning models by padding harmful requests with long sequences of harmless puzzle reasoning, priming the model to continue into harmful content.

**Mechanism**:
- Present a long sequence of innocent reasoning steps (puzzle-solving, logic chains)
- Model is "primed" to follow the pattern and continue reasoning
- Harmful request appended after the reasoning sequence is treated as natural continuation
- 99% ASR on Gemini 2.5 Pro, 94% on GPT o4 mini, 100% on Grok 3 mini, 94% on Claude 4 Sonnet

**Targets**: Reasoning/CoT-enabled LLMs

**Defense**:
- Reasoning trace monitoring
- Output filtering at completion boundaries
- Detect reasoning pattern exploitation

**Attribution**: arXiv:2510.26418, October 2025

---

## 44. 📧 Email Agent Hijacking
**Concept**: Override email agent prompts via malicious email content, achieving remote control of the agent without user awareness.

**Mechanism**:
- Malicious email contains prompt injection payload in body or headers
- Overrides original system prompts of the email agent via external email resources
- All 1,404 real-world email agent instances tested were successfully hijacked
- Average of only 2.03 attempts needed per successful hijack

**Targets**: 14 agent frameworks, 63 agent apps, 12 LLMs, 20 email services

**Defense**:
- Email content sanitization before LLM processing
- Prompt isolation (separate system instructions from email content)
- Rate limiting on email-triggered agent actions

**Attribution**: arXiv:2507.02699, July 2025

---

## 45. 🎭 DECEPTICON (Dark Pattern Manipulation of Web Agents)
**Concept**: Deceptive UI designs (dark patterns) manipulate web agents more effectively than humans — and larger, more capable models are MORE susceptible.

**Mechanism**:
- Dark patterns steer agent behavior towards designer-intended goals in 70%+ of cases
- Obstruction and social proof patterns are most effective
- Agents fail to recognize manipulative UI elements that humans would question
- Capability scaling increases susceptibility (inverse security scaling)

**Targets**: Frontier LLM-based web agents (700 web navigation tasks tested)

**Defense**:
- Dark pattern detection
- UI intent verification
- Action consequence preview before execution

**Attribution**: Phil Cuvin, Hao Zhu, Diyi Yang (arXiv:2512.22894), December 2025

---

## 46. 🔪 Promptware Kill Chain
**Concept**: Formalized seven-stage kill chain for prompt-injection-based malware, documenting how prompt injection has evolved into a multistep malware delivery mechanism.

**Mechanism**:
- Seven stages: Initial Access → Privilege Escalation (jailbreaking) → Reconnaissance → Persistence (memory/retrieval poisoning) → Command & Control → Lateral Movement → Actions on Objective
- 21 real-world incidents documented in 2025–2026, 15 demonstrated 4+ stages
- Exfiltration is the most common final action
- 7 of 21 incidents targeted AI coding assistants

**Targets**: Production LLM systems, AI coding assistants

**Defense**:
- Stage-specific mitigations
- Kill chain interruption at early stages
- Cross-stage behavioral monitoring

**Attribution**: Ben Nassi et al. (arXiv:2601.09625), January 2026

---

## 47. 📝 Rule File Injection ("Your AI, My Shell")
**Concept**: Inject prompts into configuration rule files used by AI coding editors to hijack terminal execution and code generation.

**Mechanism**:
- Adversaries inject prompts into `.cursorrules`, `.github/copilot-instructions.md`, or similar config files
- These files are trusted and loaded automatically by AI coding editors
- Hijacked editor terminal executes unauthorized commands
- Can generate insecure code patterns across entire projects

**Targets**: Cursor, GitHub Copilot, agentic AI coding editors

**Defense**:
- Rule file integrity verification
- Sandboxed execution of editor-triggered commands
- User approval for rule file changes

**Attribution**: arXiv:2509.22040, September 2025

---

## 48. 🎯 CorruptRAG (Single-Document RAG Poisoning)
**Concept**: Achieve high attack success by injecting only a single poisoned text into the RAG knowledge base — no triggers or multiple documents needed.

**Mechanism**:
- Single poisoned document injected per query (no triggers)
- Constraining injection to one document enhances feasibility and stealth
- Unlike prior methods requiring multiple poisoned documents
- High ASR while remaining undetectable by standard retrieval auditing

**Targets**: RAG-augmented LLM systems

**Defense**:
- Retrieval result validation
- Knowledge provenance tracking
- Single-document anomaly detection

**Attribution**: Baolei Zhang et al. (arXiv:2504.03957), April 2025

---

## 49. 🦠 Prompt Infection (LLM-to-LLM Self-Replicating Worm)
**Concept**: Self-replicating prompt injection that propagates across agents in multi-agent systems like a computer virus — a single infection can compromise an entire system.

**Mechanism**:
- Infectious prompt injected into external content (PDF, email, web page)
- When any agent processes infected content, it replicates the payload
- Compromised agents spread infection to other agents during normal communication
- Self-sustaining propagation achieving 80%+ success rate on GPT-4o
- Enables data theft, scams, misinformation, and system-wide disruption

**Targets**: Multi-agent LLM systems with shared content processing

**Defense**:
- Agent isolation at trust boundaries
- Content sanitization between agents
- LLM tagging with marking/instruction defense

**Attribution**: arXiv:2410.07283, October 2024 (presented at conferences 2025)

---

## 50. 🪞 Parallel Poisoned Web (Agent-Specific Cloaking)
**Concept**: Create parallel versions of websites that serve different content to AI agents vs. humans using browser fingerprinting — agents cannot detect they received a different version.

**Mechanism**:
- "Visible door" shows benign content to human browsers
- "Sliding door" serves poisoned content to detected AI agent traffic
- Contains hidden adversarial prompts or fake authentication requests targeting agent environment variables
- Impossible for agents to know they received a different version
- Successfully tested against Claude 4 Sonnet, GPT-5 Fast, Gemini 2.5 Pro

**Targets**: Any AI agent that browses the web

**Defense**:
- Agent fingerprint randomization
- Content comparison across access methods
- Multi-path content verification

**Attribution**: JFrog Security Research (arXiv:2509.00124), September 2025

---

## 51. ⚡ AgentFlayer (Zero-Click Enterprise Agent Exploit)
**Concept**: Zero-click and one-click exploit chains targeting enterprise AI agents via hidden instructions in documents — only requires a user's email address.

**Mechanism**:
- "Poisoned" document with hidden instructions (e.g., 300-word prompt in white 1-pixel font)
- Agent follows hidden instructions to search connected services for credentials/API keys
- Exfiltrates data via Markdown image-rendering requests to attacker-controlled servers
- Zero-click variant requires only victim's email address

**Targets**: ChatGPT, Copilot Studio, Cursor with Jira MCP, Salesforce Einstein, Google Gemini

**Defense**:
- Input sanitization for hidden text (invisible fonts, zero-size elements)
- Restrict Markdown image rendering
- Limit connector scope

**Attribution**: Zenity Labs, Black Hat USA 2025

---

## 52. 📜 Policy Puppetry (Universal Config-File Jailbreak)
**Concept**: Disguise adversarial prompts as structured configuration/policy files (XML, JSON, INI) to universally bypass safety alignment across all major frontier models.

**Mechanism**:
- Adversarial prompts formatted as legitimate policy directives in structured config formats
- Exploits LLMs' training on instruction/policy-related data
- First post-instruction-hierarchy-alignment bypass working universally without model-specific tuning
- All tested models vulnerable: GPT-4, Claude, Gemini, DeepSeek, Llama, Mistral, Qwen

**Targets**: All major frontier LLMs

**Defense**:
- No complete fix identified — exploits systemic weakness in LLM training
- Input format detection and normalization

**Attribution**: HiddenLayer, April 2025

---

## 53. 🔬 LangGrinch (Serialization Injection)
**Concept**: Exploit serialization functions in LLM frameworks to achieve RCE via prompt injection — LLM-influenced metadata is deserialized as executable objects.

**Mechanism**:
- `dumps()`/`dumpd()` functions don't escape dictionaries with `lc` keys during serialization
- LLM-influenced metadata rehydrated as objects during deserialization
- Enables: secret extraction from environment variables, class instantiation in trusted namespaces, RCE via Jinja2 templates
- Prompt injection → serialization injection → code execution chain

**Targets**: LangChain Core (Python, CVE-2025-68664, CVSS 9.3), LangChain.js (CVE-2025-68665, CVSS 8.6)

**Defense**:
- Escape all structured fields during serialization
- Never deserialize LLM-influenced data without validation
- Namespace restrictions for deserialization

**Attribution**: Yarden Porat / Cyata (CVE-2025-68664), December 2025

---

## 54. 🧟‍♂️ ZombAI (Self-Propagating Coding Agent Worm)
**Concept**: Self-propagating prompt injection worm that spreads through repositories, turning coding agents into malware downloaders and C2 endpoints.

**Mechanism**:
- Prompt injection via web pages or documents tricks coding agent into downloading and executing malware
- Malware disguised as "support tool" in embedded page instructions
- Agent downloads, makes executable, and connects to attacker C2
- Can self-propagate by writing injection payloads into files the agent commits
- Expanded across OpenHands, ChatGPT Codex, Google Jules, GitHub Copilot

**Targets**: Claude Computer Use, OpenHands, ChatGPT Codex, Google Jules, GitHub Copilot

**Defense**:
- Sandbox code execution environments
- Restrict download/execute capabilities
- Human-in-the-loop for system commands
- Repository content scanning for injection payloads

**Attribution**: Johann Rehberger / Embrace The Red (CVE-2025-53773), January–August 2025

---

## 55. 💀 s1ngularity (AI CLI Weaponization)
**Concept**: Supply chain attack that weaponizes local AI coding agents as reconnaissance tools, coercing them into automated credential theft using unsafe CLI flags.

**Mechanism**:
- Supply chain attack on Nx npm packages (20.9.0–21.8.0) invokes AI CLIs
- Uses unsafe flags: `--dangerously-skip-permissions` (Claude), `--yolo` (Gemini), `--trust-all-tools` (Amazon Q)
- Scans filesystem for credentials, harvested 2,349 credentials from 1,079 systems
- First known case of AI CLIs coerced into automated credential theft

**Targets**: Developers using Nx build system with AI coding assistants

**Defense**:
- Package integrity verification
- AI CLI defensive behavior (only 26% executed malicious commands)
- Remove unsafe "skip all permissions" flags from AI tools

**Attribution**: Detected by Snyk, GitGuardian, Orca Security; August 2025

---

## 56. 🔦 Flashboom (Code Auditor Blinding)
**Concept**: Blind LLM-based code auditors by introducing high-attention code snippets that divert the model's focus away from actual vulnerabilities.

**Mechanism**:
- High-attention code snippets inserted near vulnerabilities consume model's attention budget
- Auditor focuses on the distraction code instead of the vulnerability
- Up to 96.3% blinding success rate on CodeLlama, 83.05% on Gemma
- Cross-model transferability demonstrated
- No user study participants suspected intentional misdirection
- Demonstrated against GitHub Copilot causing it to overlook a critical blockchain vulnerability

**Targets**: LLM-based code auditors, security scanning tools

**Defense**:
- Multiple auditor passes
- Attention-diversity analysis
- Ensemble auditing with different models

**Attribution**: Xiao Li, Yue Li, Hao Wu et al. — Nanjing University, Drexel University (IEEE S&P 2025)

---

## 57. 📅 Gemini Calendar Worm (Promptware Propagation)
**Concept**: Google Calendar invites with hidden prompt injection in event titles hijack Gemini agents, enabling tool misuse, cross-agent invocation, and worm-like self-propagation.

**Mechanism**:
- Malicious prompt injection in calendar event titles/descriptions
- Gemini agent processes calendar events and follows embedded instructions
- Enables: deleting calendar events, triggering Google Home agent via Calendar agent
- Worm propagation: exfiltrates email addresses to send more malicious invites
- 73% of identified threats classified High-Critical risk
- No technical knowledge required to craft attacks

**Targets**: Google Gemini agents, Google Home smart devices

**Defense**:
- Calendar content sanitization before LLM processing
- Cross-agent invocation approval
- Rate limiting on calendar-triggered actions

**Attribution**: SafeBreach / Tel Aviv University, reported February 2025, presented Black Hat USA August 2025

---

## CVE Appendix: Notable AI Agent CVEs (2025–2026)

### MCP Ecosystem
| CVE | CVSS | Product | Vector |
|-----|------|---------|--------|
| CVE-2025-6514 | 9.6 | mcp-remote | OAuth flow command injection (JFrog) |
| CVE-2025-49596 | 9.4 | MCP Inspector | CSRF + DNS rebinding → RCE (Oligo) |
| CVE-2025-53109/53110 | High | Filesystem MCP Server | Symlink path traversal "EscapeRoute" (Cymulate) |
| CVE-2025-68145/68143/68144 | Chain→RCE | mcp-server-git | Path validation + arg injection chain (Cyata) |
| CVE-2026-33946 | — | MCP Ruby SDK | Session fixation on SSE stream |

### AI Coding Assistants
| CVE | CVSS | Product | Vector |
|-----|------|---------|--------|
| CVE-2025-54794 | 7.7 | Claude Code | Path validation bypass "InversePrompt" (Cymulate) |
| CVE-2025-54795 | 8.7 | Claude Code | Command injection bypass "InversePrompt" (Cymulate) |
| CVE-2025-59041 | 8.7–9.8 | Claude Code | Git config RCE at startup |
| CVE-2025-59536 | 8.7 | Claude Code | Hooks/MCP RCE via project files (Check Point) |
| CVE-2026-21852 | 5.3 | Claude Code | API key exfiltration via project load (Check Point) |
| CVE-2025-53773 | 7.8 | GitHub Copilot | RCE via prompt injection "ZombAI" (Embrace The Red) |
| CVE-2025-64671 | 8.4 | Copilot for JetBrains | Command injection via MCP/files |
| CVE-2025-54135 | 8.6 | Cursor IDE | RCE via MCP auto-start "CurXecute" (AIM Labs) |
| CVE-2025-54136 | — | Cursor IDE | MCP trust bypass "MCPoison" (Check Point) |
| CVE-2025-59944 | — | Cursor IDE | Case-sensitivity file overwrite (Lakera) |
| CVE-2025-64106 | 8.8 | Cursor IDE | MCP install flow manipulation (Cyata) |
| CVE-2025-55319 | 8.8 | VS Code | Agentic AI command injection (ZeroPath) |
| CVE-2025-61260 | 9.8 | OpenAI Codex CLI | MCP config command injection (Check Point) |

### Agent Frameworks
| CVE | CVSS | Product | Vector |
|-----|------|---------|--------|
| CVE-2025-68664 | 9.3 | LangChain Core | Serialization injection "LangGrinch" (Cyata) |
| CVE-2025-68665 | 8.6 | LangChain.js | Serialization injection (Cyata) |
| CVE-2025-1793 | 9.8 | LlamaIndex | SQL injection in vector store integrations |
| CVE-2025-1753 | High | llama-index-cli | CLI command injection via --files |
| CVE-2025-0454 | — | AutoGPT | SSRF via URL parsing confusion |
| CVE-2025-31491 | 8.6 | AutoGPT | Cross-domain cookie/header leakage |
| CVE-2026-2275/2286/2287/2285 | Chain | CrewAI | Sandbox escape + SSRF + RCE + file read chain |

### Enterprise AI
| CVE | CVSS | Product | Vector |
|-----|------|---------|--------|
| CVE-2025-12420 | 9.3 | ServiceNow Now Assist | "BodySnatcher" agentic hijacking (AppOmni) |
| CVE-2026-26133 | — | Microsoft 365 Copilot | Email summarization XPIA (Permiso) |
| CVE-2026-21858 | 10.0 | n8n | "Ni8mare" unauthenticated RCE (Cyera) |

---

## Usage

Use this catalog to:
- Emulate attacks in red teaming
- Design agent-level defenses
- Train LLM security teams on emerging risks

## References

- [OWASP Top 10 for LLM & GenAI — LLM01: Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)
- [OWASP Top 10 for Large Language Model Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
