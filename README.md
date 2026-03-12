# opencode-permissions

Configure OpenCode security permissions for safe development workflows.

## Overview

`opencode-permissions` is an OpenCode skill that helps you configure security permissions to control what actions require approval. Whether you're working solo or in a team, this skill provides templates and guidance for secure OpenCode configurations.

## Installation

### Global Installation (Recommended)

The skill is automatically available globally when symlinked to your OpenCode configuration:

```bash
# The skill should be symlinked to:
~/.config/opencode/skills/opencode-permissions -> ~/dev/opencode-permissions/
```

### Manual Installation

```bash
# Clone the repository
git clone https://github.com/NjengaFelix/opencode-permissions.git

# Create symlink for global access
ln -s $(pwd)/opencode-permissions ~/.config/opencode/skills/opencode-permissions
```

## Usage

Simply ask OpenCode to load the skill:

```
Load opencode-permissions
```

Or reference it when asking for help:

```
Help me set up secure permissions for my team
```

## What This Skill Covers

### 🔐 Permission Levels
- **allow** - Immediate execution (safe operations)
- **ask** - Prompt for approval (destructive operations)
- **deny** - Completely blocked

### 🎯 Security Templates
- **Production-Safe Setup** - Maximum safety for critical projects
- **Developer-Friendly Setup** - Balance of safety and speed
- **Read-Only Exploration** - Safe code investigation

### 🛡️ Advanced Patterns
- Pattern-based permissions with wildcards
- Per-agent permission overrides
- Bash command-specific controls
- MCP server permissions

## Quick Examples

### Secure Team Setup

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

### Pattern-Based Bash Permissions

```json
{
  "permission": {
    "bash": {
      "*": "ask",
      "git status*": "allow",
      "git log*": "allow",
      "npm test": "allow",
      "rm *": "deny"
    }
  }
}
```

## Directory Structure

```
opencode-permissions/
├── SKILL.md          # Main skill definition
├── README.md         # This file
└── .git/            # Git repository
```

## Development

The skill is developed in `~/dev/opencode-permissions/` and symlinked to `~/.config/opencode/skills/` for global availability.

### Making Changes

1. Edit `SKILL.md` in `~/dev/opencode-permissions/`
2. Changes are immediately available (symlink is live)
3. Commit your changes:
   ```bash
   cd ~/dev/opencode-permissions
   git add .
   git commit -m "Your commit message"
   git push origin main
   ```

## Use Cases

- **Team Security** - Set up permissions for collaborative development
- **Production Safety** - Prevent accidental destructive operations
- **Compliance** - Meet organizational security requirements
- **Permission Debugging** - Fix permission-related issues
- **Command Control** - Fine-grained bash command access

## Best Practices

1. **Start Restrictive** - Begin with `ask`, relax as needed
2. **Use Deny Carefully** - Only for truly dangerous operations
3. **Test Permissions** - Verify `ask` prompts appear correctly
4. **Document Exceptions** - Comment why certain commands are allowed
5. **Review Regularly** - Audit permissions as project evolves

## Security Checklist

- [ ] `write` and `edit` set to `ask` for critical projects
- [ ] `bash` has specific allowlist
- [ ] Dangerous commands (`rm -rf`, `sudo`) are denied
- [ ] MCP servers reviewed before enabling
- [ ] Team members understand permission levels
- [ ] Config reviewed before committing to git

## Related Skills

- **opencode-init** - Main initialization skill
- **opencode-mcp** - MCP server configuration
- **opencode-agents** - Custom agent creation
- **opencode-models** - Model selection and optimization

## Contributing

Contributions welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## Support

- **OpenCode Docs**: https://opencode.ai/docs
- **Discord**: https://opencode.ai/discord
- **Issues**: https://github.com/NjengaFelix/opencode-permissions/issues

## License

MIT License - See [SKILL.md](SKILL.md) for details

---

**Built with ❤️ for the OpenCode community**
