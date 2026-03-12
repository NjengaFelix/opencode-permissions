---
name: opencode-permissions
description: Configure OpenCode security permissions for safe development workflows
license: MIT
compatibility: opencode
metadata:
  category: security
  type: community
---

## What I do

I help you configure OpenCode permissions for secure development:

- **Set up permission levels** - Choose between allow, ask, deny
- **Pattern matching** - Configure permissions for specific commands
- **Team security** - Safe defaults for collaborative projects
- **Per-agent permissions** - Different rules for different agents
- **Bash command control** - Fine-grained shell access
- **Audit existing config** - Review and improve current permissions

## When to use me

Load this skill when:

- **Securing a project** - "Make this OpenCode setup production-safe"
- **Team onboarding** - "Set up permissions for our team"
- **Permission problems** - "Why isn't my permission working?"
- **Adding restrictions** - "Prevent OpenCode from running dangerous commands"
- **Compliance needs** - "Meet our security requirements"

## Permission Levels

### Three Levels of Control

| Level | Behavior | Use Case |
|-------|----------|----------|
| `allow` | Immediate execution | Safe operations (read, glob, grep) |
| `ask` | Prompt for approval | Destructive operations (write, edit, bash) |
| `deny` | Completely blocked | Untrusted operations |

### Global Permissions (opencode.json)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "skill": "ask",
    "webfetch": "allow",
    "read": "allow",
    "write": "ask",
    "edit": "ask",
    "bash": "ask",
    "glob": "allow",
    "grep": "allow"
  }
}
```

### Pattern-Based Permissions

Control specific commands with wildcards:

```json
{
  "permission": {
    "bash": {
      "*": "ask",
      "git status*": "allow",
      "git log*": "allow",
      "git diff*": "allow",
      "npm test": "allow",
      "npm run build": "ask",
      "rm *": "deny"
    },
    "skill": {
      "*": "ask",
      "documentation-*": "allow",
      "internal-*": "deny"
    }
  }
}
```

**Important:** Last matching rule wins. Put specific rules after wildcards.

## Security Templates

### Production-Safe Setup
Maximum safety for critical projects:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "default_agent": "plan",
  "permission": {
    "skill": "ask",
    "webfetch": "allow",
    "read": "allow",
    "write": "ask",
    "edit": "ask",
    "bash": "ask",
    "glob": "allow",
    "grep": "allow"
  }
}
```

### Developer-Friendly Setup
Balance of safety and speed:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "default_agent": "build",
  "permission": {
    "skill": "allow",
    "webfetch": "allow",
    "read": "allow",
    "write": "allow",
    "edit": "allow",
    "bash": {
      "*": "ask",
      "git *": "allow",
      "npm test": "allow",
      "npm run lint": "allow"
    },
    "glob": "allow",
    "grep": "allow"
  }
}
```

### Read-Only Exploration
Safe for investigating code:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "default_agent": "plan",
  "permission": {
    "skill": "ask",
    "webfetch": "allow",
    "read": "allow",
    "write": "deny",
    "edit": "deny",
    "bash": "deny",
    "glob": "allow",
    "grep": "allow"
  }
}
```

## Per-Agent Permissions

Different agents, different rules:

```json
{
  "agent": {
    "build": {
      "permission": {
        "write": "allow",
        "edit": "allow",
        "bash": "ask"
      }
    },
    "plan": {
      "permission": {
        "write": "deny",
        "edit": "deny",
        "bash": "deny"
      }
    },
    "review": {
      "permission": {
        "write": "deny",
        "edit": "deny",
        "bash": {
          "git *": "allow",
          "*": "deny"
        }
      }
    }
  }
}
```

## Advanced Patterns

### Command-Specific Bash Permissions

```json
{
  "permission": {
    "bash": {
      "*": "ask",
      "git status*": "allow",
      "git log*": "allow",
      "git diff*": "allow",
      "git branch*": "allow",
      "npm test": "allow",
      "npm run lint": "allow",
      "npm run format": "allow",
      "npm run build": "ask",
      "npm install": "ask",
      "rm *": "deny",
      "sudo *": "deny",
      "*rm -rf*": "deny"
    }
  }
}
```

### Skill Permissions with Wildcards

```json
{
  "permission": {
    "skill": {
      "*": "ask",
      "documentation-*": "allow",
      "testing-*": "allow",
      "internal-*": "deny",
      "experimental-*": "ask"
    }
  }
}
```

### MCP Server Permissions

```json
{
  "permission": {
    "mcp": {
      "filesystem": "allow",
      "brave-search": "ask",
      "internal-mcp": "deny"
    }
  }
}
```

## Common Issues

### Permission not applying
**Problem:** Configured `ask` but it's allowing
**Solution:** Check hierarchy - agent config overrides global

### Wildcards not working
**Problem:** `git*` doesn't match `git status`
**Solution:** Use `git *` (with space) not `git*`

### Can't override specific command
**Problem:** Global `*` rule blocks everything
**Solution:** Put specific rules AFTER wildcard:
```json
{
  "bash": {
    "*": "ask",
    "git status": "allow"
  }
}
```

## Best Practices

1. **Start restrictive** - Begin with `ask`, relax as you trust the AI
2. **Use deny carefully** - Only for truly dangerous operations
3. **Test permissions** - Verify `ask` prompts appear correctly
4. **Document exceptions** - Comment why certain commands are allowed
5. **Review regularly** - Audit permissions as project evolves

## Security Checklist

- [ ] `write` and `edit` set to `ask` or `deny` for critical projects
- [ ] `bash` has specific allowlist, not just `allow`
- [ ] Dangerous commands (`rm -rf`, `sudo`) are denied
- [ ] MCP servers reviewed before enabling
- [ ] Team members understand permission levels
- [ ] Config reviewed before committing to git
