# 🔨 Forge

**An Agno-Powered, Local-First AI Coding Platform for Spec-Driven Development.**

Forge is an open-source, terminal-based AI software development platform. Built on the [Agno](https://github.com/agno-agi/agno "null") multi-agent framework, Forge acts as a paradigm evolution from tools like OpenCode. It is explicitly designed to orchestrate complex software development lifecycles (SDLC) entirely locally using consumer hardware.

By combining `llama.cpp` hybrid inference, strict Spec-Driven Development (SDD) pipelines, and the Model Context Protocol (MCP), Forge turns a single machine into an autonomous, hierarchical AI development studio.

## ✨ Why Forge?

Current IDE agents and CLI coding assistants suffer from three critical bottlenecks:

1. **The Reasoning Model Crash:** Traditional coding assistants crash when trying to parse tool-calls (JSON) mixed with `<think>` blocks from reasoning models like `DeepSeek-R1` or `Phi-4`.
    
2. **Monolithic Agents:** Relying on a single massive model to do everything (brainstorm, code, save files, and test) is slow, hallucinates frequently, and bottlenecks VRAM.
    
3. **Weak Constraints:** LLMs often ignore system prompts instructing them to "write specs before coding."
    

**Forge solves this:**

- **Native CoT Parsing:** Agno natively intercepts and strips `<think>` blocks before validation, allowing reasoning models to execute tools flawlessly.
    
- **Hierarchical Swarms:** Work flows through a strict state machine. An Architect agent generates a spec, a Planner breaks it down, and a Builder executes it. They cannot bypass the hierarchy.
    
- **Smart Hardware Routing:** Tasks are dynamically routed to 14B, 30B, or 3B models based on cognitive load, maximizing token generation on a 12GB VRAM GPU.
    

## 🏗️ Architecture & The Swarm

Forge relies on a multi-tiered agent architecture, orchestrated via Agno `Workflows` and `Teams`.

### Tier 1: The Thinkers (14B)

- **Model:** `unsloth/DeepSeek-R1-Distill-Qwen-14B-GGUF`
    
- **Agents:** `@brainstorm`, `@architect`, `@test`, `@plan`, `@synthesize`
    
- **Role:** CoT reasoning, ambiguity resolution, adversarial QA testing, and systems design. Fits entirely in VRAM for blazing-fast thinking.
    

### Tier 2: The Executors (30B)

- **Model:** `unsloth/Qwen3-Coder-30B-A3B-Instruct-GGUF`
    
- **Agents:** `@build`, `@codify`
    
- **Role:** Flawless syntax generation and MCP file manipulation. Operates via **Hybrid CPU+GPU Offloading** to leverage high-bandwidth DDR5 system RAM alongside the GPU.
    

### Tier 3: The Utility Scripts (3B)

- **Model:** `unsloth/Qwen2.5-Coder-3B-Instruct-GGUF`
    
- **Agents:** `@scribe`, `@audit`, `@scout`
    
- **Role:** Sub-agents acting as background utilities for mechanical tasks (saving markdown files, running `grep` audits, summarizing web searches) with zero reasoning overhead.
    

## 🚀 Key Features

- **Cognitive Scratchpad (`todowrite`):** A custom tool and UI pane that forces the AI to construct a physical checklist before taking action, anchoring its context window and preventing task drift.
    
- **Textual TUI:** A split-pane, keyboard-first terminal user interface written in Python. Watch the swarm's CoT reasoning, task checklists, and code diffs in real-time.
    
- **MCP Integration:** Forge uses the Model Context Protocol (MCP) to read local file systems, query databases, and fetch documentation without writing brittle custom Python tools.
    
- **Local Wiki Graph:** Agents natively maintain an Obsidian-style `[[Wiki-Link]]` directory to track project architecture, eliminating the need to read the entire codebase on every prompt.
    

## 💻 System Requirements

Forge is heavily optimized for mid-to-high-end consumer rigs utilizing `llama.cpp` (`llama-server`).

- **Recommended Minimum Hardware:**
    
    - **GPU:** RTX 3060 12GB VRAM (or equivalent)
        
    - **RAM:** 32GB DDR5 (Crucial for 30B model hybrid offloading)
        
    - **CPU:** Ryzen 7 7700 (or equivalent modern multi-core CPU)
        

## 🛠️ Getting Started (WIP)

_Note: Forge is currently in active development. The steps below reflect the intended setup process._

**1. Start the Inference Router**

Ensure you have `llama.cpp` installed and your GGUF models downloaded.

```
# Start the llama-server with the multi-model configuration
llama-server --config forge-models.json --mmap --flash-attn -c 32768
```

**2. Install Forge**

```
git clone [https://github.com/yourusername/forge.git](https://github.com/yourusername/forge.git)
cd forge
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

**3. Launch the TUI**

```
python -m forge.cli
```

## 🛣️ Roadmap

- [x] Agent Markdown Manifests & Pipeline Design
    
- [x] Agno Hardware Orchestration Strategy
    
- [ ] Step 1: Implement `CognitiveScratchpad` and Agno Workflow State Machine.
    
- [ ] Step 2: Integrate `local-filesystem` MCP server.
    
- [ ] Step 3: Build the Textual TUI prototype (Chat, Scratchpad, Workspace Tree).
    
- [ ] Step 4: Alpha Release for local testing.
    

## 🤝 Contributing

Contributions are welcome! If you're interested in building custom MCP servers for Forge, improving the Textual UI, or writing new Skill runbooks, please open an issue or submit a pull request.

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](https://www.google.com/search?q=LICENSE "null") file for details.
