# High-Level System Architecture

This diagram shows the overall structure of Claude Code's prompt system.

```mermaid
flowchart TB
    subgraph Core["Core System"]
        Main["<b>Main System Prompt</b><br/>Behavior, tone, policies,<br/>task handling"]
        click Main href "../system-prompts/system-prompt-main-system-prompt.md" "Main System Prompt"
    end

    subgraph Conditional["Conditional Additions"]
        Security["Security Policy"]
        Learning["Learning Mode"]
        Git["Git Status"]
        Scratchpad["Scratchpad Directory"]
        MCP["MCP CLI"]
        Chrome["Chrome Browser"]
        click Learning href "../system-prompts/system-prompt-learning-mode.md" "Learning Mode"
        click Git href "../system-prompts/system-prompt-git-status.md" "Git Status"
        click Scratchpad href "../system-prompts/system-prompt-scratchpad-directory.md" "Scratchpad Directory"
        click MCP href "../system-prompts/system-prompt-mcp-cli.md" "MCP CLI"
        click Chrome href "../system-prompts/system-prompt-claude-in-chrome-browser-automation.md" "Chrome Browser Automation"
    end

    subgraph Tools["Built-in Tools"]
        direction LR
        FileTools["File Operations<br/>Read, Write, Edit,<br/>Glob, Grep"]
        ExecTools["Execution<br/>Bash"]
        WebTools["Web<br/>Search, Fetch"]
        PlanTools["Planning<br/>TodoWrite,<br/>Enter/Exit PlanMode"]
        InteractTools["Interaction<br/>Task, Skill,<br/>AskUserQuestion"]
    end

    subgraph Agents["Sub-agents"]
        direction LR
        Explore["Explore<br/><i>Read-only search</i>"]
        Plan["Plan<br/><i>Read-only design</i>"]
        TaskAgent["Task Agent<br/><i>Autonomous work</i>"]
        click Explore href "../system-prompts/agent-prompt-explore.md" "Explore Agent"
        click Plan href "../system-prompts/agent-prompt-plan-mode-enhanced.md" "Plan Mode Agent"
        click TaskAgent href "../system-prompts/agent-prompt-task-tool.md" "Task Tool Agent"
    end

    subgraph Utilities["Utility Agents"]
        direction LR
        Summary["Conversation<br/>Summarization"]
        Session["Session Title<br/>& Branch Gen"]
        Hooks["Hooks"]
        Sentiment["User Sentiment<br/>Analysis"]
        click Summary href "../system-prompts/agent-prompt-conversation-summarization.md" "Conversation Summarization"
        click Session href "../system-prompts/agent-prompt-session-title-and-branch-generation.md" "Session Title Generation"
        click Hooks href "../system-prompts/agent-prompt-agent-hook.md" "Agent Hook"
        click Sentiment href "../system-prompts/agent-prompt-user-sentiment-analysis.md" "User Sentiment Analysis"
    end

    Main --> Conditional
    Main --> Tools
    Main --> Agents
    Main --> Utilities

    style Core fill:#e1f5fe
    style Tools fill:#f3e5f5
    style Agents fill:#e8f5e9
    style Utilities fill:#fff3e0
    style Conditional fill:#fce4ec
```

## Components

| Component | Description | Key Prompts |
|-----------|-------------|-------------|
| **Core System** | The main system prompt that defines Claude Code's behavior, tone, and policies | [Main System Prompt](../system-prompts/system-prompt-main-system-prompt.md) |
| **Conditional Additions** | Context-dependent prompt sections added based on environment and configuration | [Learning Mode](../system-prompts/system-prompt-learning-mode.md), [Git Status](../system-prompts/system-prompt-git-status.md), [MCP CLI](../system-prompts/system-prompt-mcp-cli.md) |
| **Built-in Tools** | Tool descriptions injected when tools are available | See [Tool Ecosystem](./3-tool-ecosystem.md) |
| **Sub-agents** | Specialized agents spawned for complex tasks | [Explore](../system-prompts/agent-prompt-explore.md), [Plan](../system-prompts/agent-prompt-plan-mode-enhanced.md), [Task](../system-prompts/agent-prompt-task-tool.md) |
| **Utility Agents** | Background agents for session management and analysis | [Summarization](../system-prompts/agent-prompt-conversation-summarization.md), [Session Title](../system-prompts/agent-prompt-session-title-and-branch-generation.md) |
