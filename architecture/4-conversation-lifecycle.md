# Conversation Lifecycle

This diagram shows how different prompts are used throughout a Claude Code session, from start to finish.

```mermaid
sequenceDiagram
    autonumber
    participant User
    participant CC as Claude Code
    participant Main as Main Agent
    participant Tools as Tools
    participant SubAgent as Sub-agents
    participant Utility as Utility Agents

    Note over CC: Session Start
    
    rect rgb(225, 245, 254)
        Note over Main: System Prompt Assembly
        CC->>Main: Load Main System Prompt
        CC->>Main: Inject Tool Descriptions
        CC->>Main: Add Conditional Sections<br/>(Git Status, Learning Mode, etc.)
    end

    User->>CC: Initial message
    CC->>Main: Process request

    rect rgb(232, 245, 233)
        Note over Main,Tools: Work Phase
        loop Task Execution
            Main->>Tools: Use tools (Read, Write, Bash, etc.)
            Tools-->>Main: Results
            
            alt Complex task
                Main->>SubAgent: Delegate via Task tool
                Note over SubAgent: Agent-specific prompt loaded
                SubAgent-->>Main: Results
            end
        end
    end

    rect rgb(255, 243, 224)
        Note over Utility: Context Management
        alt Context grows large
            CC->>Utility: Trigger Summarization
            Note over Utility: Conversation Summarization prompt
            Utility-->>CC: Compressed context
        end
    end

    User->>CC: Follow-up messages
    Note over Main: Continue with context

    rect rgb(252, 228, 236)
        Note over Utility: Session End
        CC->>Utility: Generate session title
        Note over Utility: Session Title & Branch prompt
        Utility-->>CC: Title and branch name
    end
```

## Lifecycle Phases

### 1. Session Initialization

When a Claude Code session starts, the system prompt is assembled from multiple pieces:

```mermaid
flowchart LR
    subgraph Assembly["System Prompt Assembly"]
        direction TB
        Base["Main System Prompt"]
        Tools["Tool Descriptions"]
        Cond["Conditional Sections"]
        
        Base --> Final["Complete System Prompt"]
        Tools --> Final
        Cond --> Final
    end
    
    click Base href "../system-prompts/system-prompt-main-system-prompt.md" "Main System Prompt"
```

**Key Prompts:**
- [Main System Prompt](../system-prompts/system-prompt-main-system-prompt.md) - Core behavior and policies
- [Git Status](../system-prompts/system-prompt-git-status.md) - Repository state (if in git repo)
- [Learning Mode](../system-prompts/system-prompt-learning-mode.md) - Educational mode (if enabled)
- [Scratchpad Directory](../system-prompts/system-prompt-scratchpad-directory.md) - Temp file location (if configured)

### 2. Work Phase

During active work, the main agent uses tools and may delegate to sub-agents:

| Action | Prompts Used |
|--------|-------------|
| Direct tool use | Individual [tool descriptions](./3-tool-ecosystem.md) |
| Codebase exploration | [Explore Agent](../system-prompts/agent-prompt-explore.md) via Task |
| Planning | [Plan Agent](../system-prompts/agent-prompt-plan-mode-enhanced.md) via Task |
| Task management | [TodoWrite](../system-prompts/tool-description-todowrite.md) |

### 3. Context Management

As conversation grows, summarization preserves important context:

```mermaid
flowchart LR
    Long["Long Conversation"] --> Trigger{"Context<br/>too large?"}
    Trigger -->|Yes| Summary["Summarization Agent"]
    Summary --> Compact["Compacted Context"]
    Compact --> Continue["Continue Session"]
    Trigger -->|No| Continue
    
    click Summary href "../system-prompts/agent-prompt-conversation-summarization.md" "Summarization"
```

**Key Prompts:**
- [Conversation Summarization](../system-prompts/agent-prompt-conversation-summarization.md) - Standard summarization
- [Summarization with Additional Instructions](../system-prompts/agent-prompt-conversation-summarization-with-additional-instructions.md) - Custom summarization rules

### 4. Session End

When a session concludes, utility agents generate metadata:

**Key Prompts:**
- [Session Title & Branch Generation](../system-prompts/agent-prompt-session-title-and-branch-generation.md) - Creates descriptive title and git branch name
- [Session Notes Template](../system-prompts/agent-prompt-session-notes-template.md) - Structure for session notes
- [Session Notes Update](../system-prompts/agent-prompt-session-notes-update-instructions.md) - How to update notes during session

## Prompt Injection Points

| Phase | Injection Point | Prompt Type |
|-------|----------------|-------------|
| Start | System message | Main + conditionals |
| Tool call | Tool schema | Tool descriptions |
| Sub-agent spawn | New context | Agent prompts |
| Context compact | Background | Summarization prompt |
| Plan mode | System reminder | Plan mode reminders |
| Session end | Background | Title/branch generation |
