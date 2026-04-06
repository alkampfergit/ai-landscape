# Writing effective tools for AI agents—using AI agents

- **URL**: https://www.anthropic.com/engineering/writing-tools-for-agents
- **Added**: 2026-04-06
- **Category**: reference

## Summary

Anthropic's engineering blog post on designing, describing, and optimizing the tools that AI agents invoke. The central thesis is that agents are only as effective as the tools they are given, and that tool quality is primarily a matter of three things: prompt-engineering tool descriptions and specs (which are loaded into agent context and collectively steer tool-calling behavior), managing context efficiency in tool responses (pagination, truncation, filtering with sensible defaults), and using evaluation-driven development to iteratively refine tools. The article demonstrates that even small refinements to tool descriptions can yield dramatic benchmark improvements — Claude Sonnet 3.5 achieved state-of-the-art SWE-bench Verified performance after precise tool-description edits alone. Anthropic also describes using Claude itself to optimize its own tool descriptions.

## Key Insights

- Prompt-engineer tool descriptions as carefully as system prompts — they are loaded into agent context and collectively steer tool-calling behavior
- Small tool-description refinements can yield dramatic improvements; SWE-bench Verified SOTA was achieved through tool-description edits alone
- Return natural-language identifiers (names, titles) rather than cryptic technical IDs — agents handle natural language significantly better
- Implement pagination, range selection, filtering, and truncation with sensible defaults for any tool that can return large responses; steer agents toward token-efficient strategies (many small targeted searches over one broad search)
- Prompt-engineer error responses to communicate specific, actionable corrections rather than opaque error codes or tracebacks
- Expose a `response_format` enum parameter (e.g., "concise" vs. "detailed") so agents can control response verbosity based on their needs
- Keep tool responses under ~25,000 tokens for optimal agent performance
- Namespace tools clearly to help agents distinguish between similar capabilities
- Design tools for composability — effective tools can be combined in diverse workflows the designer didn't anticipate
- Use evaluation-driven development: build targeted evals, measure tool-calling success rates, and iterate on descriptions and specs based on data
- Re-orient from deterministic software design to non-deterministic patterns — agents will call tools in unexpected orders and combinations
- Use Claude itself to optimize tool descriptions — the model can suggest improvements to its own tool specs

## Template Documents Updated

- none — this article addresses tool/MCP-server API implementation, not repository-level harness configuration, skill design, or workflow patterns covered by the template
