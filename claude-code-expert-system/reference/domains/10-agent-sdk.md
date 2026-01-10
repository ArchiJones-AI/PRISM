---
domain: agent-sdk
semantic_triggers: agent sdk, claude agent sdk, sdk, build agents, programmatic, python sdk, typescript sdk, ClaudeAgentOptions, query function, ClaudeSDKClient, custom agents, agent development, claude code sdk, pip install, npm install, bedrock, vertex, foundry, ANTHROPIC_API_KEY
priority_sources:
  - https://platform.claude.com/docs/en/agent-sdk/overview
  - https://github.com/anthropics/claude-agent-sdk-python
  - https://github.com/anthropics/claude-agent-sdk-demos
  - https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk
related_domains:
  - 02-subagents.md
  - 08-headless.md
  - 04-hooks.md
  - 03-skills.md
---

# Claude Agent SDK Reference

## Quick Answer

The **Claude Agent SDK** (formerly Claude Code SDK) lets you build custom AI agents programmatically using the same infrastructure that powers Claude Code. Available for Python and TypeScript. Use `query()` for simple text generation or `ClaudeSDKClient` for full agentic control with tools, subagents, and session management. Supports Anthropic API, AWS Bedrock, Google Vertex AI, and Azure Foundry.

## Key Concepts

### SDK vs CLI

| Aspect | Claude Code CLI | Agent SDK |
|--------|-----------------|-----------|
| Interface | Terminal/interactive | Programmatic (Python/TS) |
| Use case | Developer productivity | Building custom agents |
| Control | Manual/conversational | Full programmatic control |
| Deployment | Local development | Production systems |

### Two Main Interfaces

| Interface | Purpose | When to Use |
|-----------|---------|-------------|
| `query()` | Simple text generation | One-shot prompts, no tools needed |
| `ClaudeSDKClient` | Full agentic control | Tools, sessions, streaming, complex workflows |

### Authentication Methods

| Provider | Environment Variable | Setup |
|----------|---------------------|-------|
| **Anthropic API** | `ANTHROPIC_API_KEY` | Get from console.anthropic.com |
| **AWS Bedrock** | `CLAUDE_CODE_USE_BEDROCK=1` | Configure AWS credentials |
| **Google Vertex** | `CLAUDE_CODE_USE_VERTEX=1` | Configure Google Cloud credentials |
| **Azure Foundry** | `CLAUDE_CODE_USE_FOUNDRY=1` | Configure Azure credentials |

### Available Tools

The SDK provides access to all Claude Code tools:
- `Read` — Read file contents
- `Write` — Write/create files
- `Edit` — Edit existing files
- `Bash` — Run shell commands
- `Glob` — Find files by pattern
- `Grep` — Search file contents
- `WebSearch` — Search the web
- `WebFetch` — Fetch web pages

## Authoritative Sources

| Source | Type | URL |
|--------|------|-----|
| SDK Overview | Official Docs | https://platform.claude.com/docs/en/agent-sdk/overview |
| Python SDK | GitHub | https://github.com/anthropics/claude-agent-sdk-python |
| SDK Demos | GitHub | https://github.com/anthropics/claude-agent-sdk-demos |
| Building Agents | Blog | https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk |
| NPM Package | NPM | https://www.npmjs.com/package/@anthropic-ai/claude-agent-sdk |

## Installation

### Python
```bash
pip install claude-agent-sdk

# Note: CLI is bundled with the package
# No separate Claude Code installation needed!
```

### TypeScript/JavaScript
```bash
npm install @anthropic-ai/claude-agent-sdk
# or
yarn add @anthropic-ai/claude-agent-sdk
```

### Verify Installation
```python
import claude_agent_sdk
print(claude_agent_sdk.__version__)
```

## Common Patterns

### Simple Query (Python)
```python
import anyio
from claude_agent_sdk import query

async def main():
    async for message in query(prompt="What is 2 + 2?"):
        print(message)

anyio.run(main)
```

### Query with Tools
```python
import anyio
from claude_agent_sdk import query, ClaudeAgentOptions

async def main():
    options = ClaudeAgentOptions(
        allowed_tools=["Read", "Glob", "Grep"]
    )
    
    async for message in query(
        prompt="Find all Python files and list their imports",
        options=options
    ):
        if hasattr(message, "result"):
            print(message.result)

anyio.run(main)
```

### Full Agent with ClaudeSDKClient
```python
import anyio
from claude_agent_sdk import ClaudeSDKClient, ClaudeAgentOptions

async def main():
    options = ClaudeAgentOptions(
        allowed_tools=["Read", "Write", "Bash", "Glob", "Grep"],
        permission_mode="acceptEdits",
        model="sonnet"
    )
    
    async with ClaudeSDKClient(options) as client:
        # Start a conversation
        response = await client.send("Analyse this codebase structure")
        print(response.result)
        
        # Continue the conversation
        response = await client.send("Now create a README.md")
        print(response.result)

anyio.run(main)
```

### Streaming Responses
```python
import anyio
from claude_agent_sdk import query, ClaudeAgentOptions

async def main():
    options = ClaudeAgentOptions(allowed_tools=["Read"])
    
    async for message in query(
        prompt="Explain this code",
        options=options
    ):
        if message.type == "assistant":
            print(message.content, end="", flush=True)
        elif message.type == "tool_use":
            print(f"\n[Using tool: {message.name}]")
        elif message.type == "result":
            print(f"\n\nFinal: {message.content}")

anyio.run(main)
```

### With Subagents
```python
import anyio
from claude_agent_sdk import ClaudeSDKClient, ClaudeAgentOptions

async def main():
    # Subagents from .claude/agents/ are automatically available
    options = ClaudeAgentOptions(
        allowed_tools=["Read", "Task"],  # Task enables subagents
        permission_mode="default"
    )
    
    async with ClaudeSDKClient(options) as client:
        response = await client.send(
            "Use the code-reviewer agent to review src/auth.py"
        )
        print(response.result)

anyio.run(main)
```

### With Custom System Prompt
```python
options = ClaudeAgentOptions(
    system_prompt="""You are a security analyst. 
    Focus on identifying vulnerabilities.
    Be thorough and document all findings.""",
    allowed_tools=["Read", "Grep", "Glob"]
)
```

### Research Agent Example
```python
import anyio
from claude_agent_sdk import query, ClaudeAgentOptions

async def research_topic(topic: str) -> str:
    options = ClaudeAgentOptions(
        allowed_tools=["WebSearch", "WebFetch", "Read", "Write"],
        model="opus"  # Use Opus for deep research
    )
    
    results = []
    async for message in query(
        prompt=f"""Research the topic: {topic}
        
        1. Search the web for recent information
        2. Fetch and analyse relevant pages
        3. Synthesise findings into a report
        4. Save report to research-{topic.replace(' ', '-')}.md
        """,
        options=options
    ):
        if hasattr(message, "result"):
            results.append(message.result)
    
    return "\n".join(results)

# Run
anyio.run(research_topic, "latest developments in AI agents")
```

### TypeScript Example
```typescript
import { query, ClaudeAgentOptions } from '@anthropic-ai/claude-agent-sdk';

async function main() {
    const options: ClaudeAgentOptions = {
        allowedTools: ['Read', 'Glob'],
        model: 'sonnet'
    };
    
    for await (const message of query({
        prompt: "List all TypeScript files",
        options
    })) {
        console.log(message);
    }
}

main();
```

## Prompt Templates

### Build a Code Analysis Agent
```
Create a Python agent using Claude Agent SDK that:

1. Takes a GitHub repo URL as input
2. Clones the repo to a temp directory
3. Analyses code quality (complexity, duplication, test coverage)
4. Generates a report
5. Cleans up temp directory

Include proper error handling and streaming output.
```

### Build a Documentation Agent
```
Create an agent that:
1. Reads all Python files in a directory
2. Extracts functions and classes
3. Generates docstrings for undocumented items
4. Creates a comprehensive API reference
5. Outputs as Markdown

Use ClaudeSDKClient for session persistence between steps.
```

### Build a Security Scanner
```
Create an agent that:
1. Scans code for security vulnerabilities
2. Checks dependencies for known CVEs
3. Identifies hardcoded secrets
4. Generates SARIF-format report

Use appropriate tools and Opus model for thoroughness.
```

## Troubleshooting

### CLINotFoundError
```python
# Error: CLI not found
# Solution 1: CLI is bundled, ensure package installed correctly
pip uninstall claude-agent-sdk
pip install claude-agent-sdk

# Solution 2: Specify custom CLI path
options = ClaudeAgentOptions(
    cli_path="/path/to/claude"
)
```

### Authentication Errors
```bash
# Verify API key is set
echo $ANTHROPIC_API_KEY

# For Bedrock
export CLAUDE_CODE_USE_BEDROCK=1
# Ensure AWS credentials configured

# For Vertex
export CLAUDE_CODE_USE_VERTEX=1
# Ensure Google Cloud credentials configured
```

### Tools Not Working
```python
# Ensure tools are in allowed_tools list
options = ClaudeAgentOptions(
    allowed_tools=["Read", "Write", "Bash"]  # Explicit list
)

# Or allow all tools (less secure)
options = ClaudeAgentOptions(
    permission_mode="bypassPermissions"
)
```

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `CLINotFoundError` | CLI not in PATH | Reinstall package or set `cli_path` |
| `AuthenticationError` | Invalid/missing API key | Check `ANTHROPIC_API_KEY` |
| `ToolNotAvailable` | Tool not in allowed list | Add to `allowed_tools` |
| `PermissionDenied` | Permission mode too strict | Use `acceptEdits` or `bypassPermissions` |
| `RateLimitError` | Too many requests | Implement backoff/retry |

## Best Practices

### Error Handling
```python
from claude_agent_sdk import query, ClaudeAgentError

try:
    async for message in query(prompt="..."):
        process(message)
except ClaudeAgentError as e:
    logger.error(f"Agent error: {e}")
    # Handle gracefully
```

### Resource Cleanup
```python
# Use context manager for proper cleanup
async with ClaudeSDKClient(options) as client:
    # Your code here
# Client automatically cleaned up
```

### Rate Limiting
```python
import anyio

async def rate_limited_query(prompts, rate_per_minute=10):
    delay = 60 / rate_per_minute
    for prompt in prompts:
        async for msg in query(prompt=prompt):
            yield msg
        await anyio.sleep(delay)
```

## Related Domains

- **[Subagents](02-subagents.md)** — SDK can use subagents from `.claude/agents/`
- **[Headless](08-headless.md)** — Alternative approach for automation
- **[Hooks](04-hooks.md)** — SDK respects hook configurations
- **[Skills](03-skills.md)** — SDK can leverage skills
