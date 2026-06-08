# Slash Commands & Skills

This diagram maps Claude Code's built-in slash commands and the skill system to their prompts.

```mermaid
flowchart TB
    subgraph SlashCommands["Slash Commands"]
        direction TB
        
        subgraph PR["Pull Request Commands"]
            PRComments["/pr-comments<br/><i>Fetch PR comments</i>"]
            ReviewPR["/review-pr<br/><i>Review a PR</i>"]
            click PRComments href "../system-prompts/agent-prompt-pr-comments-slash-command.md" "/pr-comments"
            click ReviewPR href "../system-prompts/agent-prompt-review-pr-slash-command.md" "/review-pr"
        end
        
        subgraph Security["Security Commands"]
            SecReview["/security-review<br/><i>Security analysis</i>"]
            click SecReview href "../system-prompts/agent-prompt-security-review-slash.md" "/security-review"
        end
        
        subgraph Memory["Memory Commands"]
            Remember["/remember<br/><i>Save learnings</i>"]
            click Remember href "../system-prompts/agent-prompt-remember-skill.md" "/remember"
        end
    end
    
    subgraph SkillSystem["Skill System"]
        SkillTool["Skill Tool"]
        SkillDesc["Skill Description"]
        CustomSkills["Custom Skills<br/><i>User-defined</i>"]
        click SkillTool href "../system-prompts/tool-description-skill.md" "Skill Tool"
    end
    
    User["👤 User"]
    User -->|"/command"| SlashCommands
    User -->|"Skill invocation"| SkillSystem
    
    style SlashCommands fill:#e1f5fe
    style SkillSystem fill:#f3e5f5
    style PR fill:#e8f5e9
    style Security fill:#fff3e0
    style Memory fill:#fce4ec
```

## Built-in Slash Commands

### `/pr-comments` - Fetch PR Comments

Fetches and displays comments from a GitHub Pull Request.

```mermaid
flowchart LR
    Cmd["/pr-comments"] --> Agent["PR Comments Agent"]
    Agent --> Fetch["Fetch from GitHub"]
    Fetch --> Display["Display comments"]
    
    click Agent href "../system-prompts/agent-prompt-pr-comments-slash-command.md" "PR Comments Agent"
```

**Prompt:** [agent-prompt-pr-comments-slash-command.md](../system-prompts/agent-prompt-pr-comments-slash-command.md)

---

### `/review-pr` - Review Pull Request

Performs a code review on a GitHub Pull Request.

```mermaid
flowchart LR
    Cmd["/review-pr"] --> Agent["PR Review Agent"]
    Agent --> Analyze["Analyze changes"]
    Analyze --> Review["Generate review"]
    
    click Agent href "../system-prompts/agent-prompt-review-pr-slash-command.md" "PR Review Agent"
```

**Prompt:** [agent-prompt-review-pr-slash-command.md](../system-prompts/agent-prompt-review-pr-slash-command.md)

---

### `/security-review` - Security Analysis

Comprehensive security review focusing on exploitable vulnerabilities (OWASP Top 10, etc.).

```mermaid
flowchart LR
    Cmd["/security-review"] --> Agent["Security Review Agent"]
    Agent --> Scan["Scan for vulnerabilities"]
    Scan --> Report["Generate report<br/>with severity levels"]
    
    click Agent href "../system-prompts/agent-prompt-security-review-slash.md" "Security Review Agent"
```

**Prompt:** [agent-prompt-security-review-slash.md](../system-prompts/agent-prompt-security-review-slash.md)

*Note: This is the largest slash command prompt at ~2610 tokens, reflecting the depth of security analysis.*

---

### `/remember` - Save Learnings

Reviews session memories and updates `CLAUDE.local.md` with recurring patterns and learnings.

```mermaid
flowchart LR
    Cmd["/remember"] --> Agent["Remember Skill Agent"]
    Agent --> Review["Review session"]
    Review --> Update["Update CLAUDE.local.md"]
    
    click Agent href "../system-prompts/agent-prompt-remember-skill.md" "Remember Skill Agent"
```

**Prompt:** [agent-prompt-remember-skill.md](../system-prompts/agent-prompt-remember-skill.md)

---

## Skill System

The Skill tool allows execution of both built-in and custom skills.

```mermaid
flowchart TB
    subgraph Invocation["Skill Invocation"]
        Direct["Direct call"]
        SlashCmd["Via slash command"]
        Auto["Automatic trigger"]
    end
    
    SkillTool["Skill Tool"]
    
    subgraph Execution["Skill Execution"]
        Load["Load skill definition"]
        Context["Inject into conversation"]
        Run["Execute skill logic"]
    end
    
    Invocation --> SkillTool
    SkillTool --> Execution
    
    click SkillTool href "../system-prompts/tool-description-skill.md" "Skill Tool"
```

**Prompt:** [tool-description-skill.md](../system-prompts/tool-description-skill.md)

---

## Quick Reference

| Command | Purpose | Prompt File |
|---------|---------|-------------|
| `/pr-comments` | Fetch GitHub PR comments | [agent-prompt-pr-comments-slash-command.md](../system-prompts/agent-prompt-pr-comments-slash-command.md) |
| `/review-pr` | Review a Pull Request | [agent-prompt-review-pr-slash-command.md](../system-prompts/agent-prompt-review-pr-slash-command.md) |
| `/security-review` | Security vulnerability analysis | [agent-prompt-security-review-slash.md](../system-prompts/agent-prompt-security-review-slash.md) |
| `/remember` | Save session learnings | [agent-prompt-remember-skill.md](../system-prompts/agent-prompt-remember-skill.md) |
| *Skill Tool* | Execute custom skills | [tool-description-skill.md](../system-prompts/tool-description-skill.md) |

## Related Prompts

These utility prompts support the slash command system:

| Prompt | Purpose | File |
|--------|---------|------|
| **CLAUDE.md Creation** | Generate project documentation | [agent-prompt-claudemd-creation.md](../system-prompts/agent-prompt-claudemd-creation.md) |
| **Agent Creation** | Create custom agents | [agent-prompt-agent-creation-architect.md](../system-prompts/agent-prompt-agent-creation-architect.md) |
| **Prompt Suggestions** | Generate follow-up prompts | [agent-prompt-prompt-suggestion-generator-v2.md](../system-prompts/agent-prompt-prompt-suggestion-generator-v2.md) |
