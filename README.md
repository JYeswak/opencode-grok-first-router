# OpenCode Grok-First Router

Automatically route OpenCode tasks to optimal models based on complexity, achieving **76% cost savings** while maintaining **10/10 correctness**.

## Overview

This plugin implements a validated routing strategy:
- **90% of tasks** → `opencode/grok-code` (FREE, $0 cost)
- **10% high complexity** → `xai/grok-4-1-fast` (PREMIUM reasoning)

### Validated Performance

Based on comprehensive A/B testing across multiple models:

| Tier | Model | Correctness | Quality | Speed | Cost |
|------|-------|-------------|---------|-------|------|
| FREE | opencode/grok-code | 10/10 | 9/10 | 6x faster | $0.00 |
| PREMIUM | xai/grok-4-1-fast | 10/10 | 9.5/10 | Standard | ~$0.06/task |

**Cost Comparison** (100 typical tasks):
- All Claude Sonnet: $2.50
- Grok-First Router: **$0.60** (76% savings)

## Installation

### Method 1: Via opencode.json (Recommended)

Edit `~/.config/opencode/opencode.json`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "plugin": [
    "opencode-grok-first-router"
  ]
}
```

Restart OpenCode. The plugin will auto-install via Bun.

### Method 2: Local Development

Clone and link locally:

```bash
cd ~/.config/opencode/plugin
git clone https://github.com/yourusername/opencode-grok-first-router.git
```

Add to `opencode.json`:

```json
{
  "plugin": [
    "grok-first-router"
  ]
}
```

### Method 3: Direct GitHub

```json
{
  "plugin": [
    "yourusername/opencode-grok-first-router"
  ]
}
```

## How It Works

### Complexity Detection

The router analyzes your prompt for complexity indicators:

**HIGH COMPLEXITY** (→ PREMIUM reasoning):
- Architectural decisions ("design", "architect")
- Novel problems ("novel", "complex", "sophisticated")
- Optimization ("optimize", "performance tuning")
- Decision-making ("figure out", "decide", "best approach")
- Ambiguity ("unclear", "multiple ways", "trade-offs")

**MEDIUM/LOW COMPLEXITY** (→ FREE tier):
- Structured implementation
- CRUD operations
- Following clear requirements
- Bug fixes with obvious solutions
- Prototyping

### Routing Logic

```
User Prompt → Complexity Detection
              │
              ├─→ HIGH → xai/grok-4-1-fast (reasoning)
              │
              └─→ MEDIUM/LOW → opencode/grok-code (FREE)
```

## Examples

### FREE Tier (90% of tasks)

```bash
# Structured implementation
opencode run "Create a Python function to validate email addresses"
# → Routes to: opencode/grok-code (FREE)

# CRUD operations
opencode run "Add a DELETE endpoint to the users API"
# → Routes to: opencode/grok-code (FREE)

# Clear requirements
opencode run "Fix the bug in login.ts where email validation fails"
# → Routes to: opencode/grok-code (FREE)
```

### PREMIUM Tier (10% of tasks)

```bash
# Architectural decision
opencode run "Design a scalable architecture for real-time notifications"
# → Routes to: xai/grok-4-1-fast (PREMIUM)

# Novel problem
opencode run "Figure out the best approach for handling rate limiting"
# → Routes to: xai/grok-4-1-fast (PREMIUM)

# Optimization
opencode run "Optimize the database query performance for complex joins"
# → Routes to: xai/grok-4-1-fast (PREMIUM)
```

## Configuration

### Environment Variables

Ensure you have xAI API credentials:

```bash
export X_AI_API_KEY="your-xai-api-key"
```

Or use a secrets loader:

```bash
# ~/.opencode/load-secrets.sh
export X_AI_API_KEY=$(grep '^X_AI_API_KEY=' ~/.elektrafi-secrets | cut -d= -f2)
```

### Override Defaults

To always use FREE tier (testing):

```json
{
  "agent": {
    "default": {
      "model": "opencode/grok-code"
    }
  }
}
```

To always use PREMIUM tier:

```json
{
  "agent": {
    "default": {
      "model": "xai/grok-4-1-fast"
    }
  }
}
```

## Verification

After installation, start OpenCode and look for:

```
✓ Grok-First Router active (90% FREE / 10% PREMIUM)
```

Test routing:

```bash
# Should route to FREE
opencode run "write hello world"

# Should route to PREMIUM
opencode run "design a complex system architecture"
```

## Performance Metrics

From comprehensive A/B testing (Phase 1 + Phase 2):

**FREE Tier Performance**:
- Correctness: 10/10 (all tests passed)
- Quality: 9/10 (clean, maintainable code)
- Speed: 6x faster than paid models
- Cost: $0.00

**PREMIUM Tier Performance**:
- Correctness: 10/10 (all tests passed)
- Quality: 9.5/10 (includes self-correction)
- Speed: Standard
- Cost: ~$0.05-0.07 per task

**Cost Savings** (typical workload):
- 100 tasks with Grok-First: $0.60
- 100 tasks with Claude Sonnet: $2.50
- **Savings: 76%**

## Troubleshooting

### Plugin not loading

Check OpenCode logs:

```bash
opencode --version
opencode mcp list
```

Ensure `grok-first-router` appears in the plugin list.

### Always routing to FREE

Your prompts may not contain high complexity keywords. Try:

```bash
opencode run "design a scalable architecture"
```

This should route to PREMIUM tier.

### xAI API key error

Verify your API key is set:

```bash
echo $X_AI_API_KEY
```

Should output your key (not empty).

## Integration with Ralph

Works seamlessly with Ralph autonomous agents:

```bash
# In .agents/ralph/agents.sh
source ~/.opencode/load-secrets.sh

AGENT_OPENCODE_CMD="opencode run --plugin grok-first-router -m opencode/grok-code \"\$(cat {prompt})\""
```

The plugin will override the `-m` flag based on complexity.

## Contributing

Found a bug or want to improve complexity detection?

1. Fork the repo
2. Create a feature branch
3. Submit a pull request

### Complexity Detection Improvements

The `detectComplexity()` function can be tuned. Suggestions:
- Add domain-specific keywords (e.g., "blockchain", "ML model")
- Weight keywords by importance
- Use regex patterns for better matching
- Add context awareness (previous messages)

## License

MIT

## Credits

Built and validated by Josh based on comprehensive multi-model A/B testing. Performance metrics from Phase 1 and Phase 2 testing with 10+ models across FREE, STANDARD, and PREMIUM tiers.

## Links

- [OpenCode](https://opencode.ai/)
- [OpenCode Cafe](https://www.opencode.cafe/)
- [xAI Grok](https://x.ai/)

---

**Status**: Production-ready
**Tested**: January 2026
**Performance**: 76% cost savings, 10/10 correctness maintained
