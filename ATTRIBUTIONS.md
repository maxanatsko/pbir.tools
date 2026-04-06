# Attributions

## Fabric CLI

For remote operations with Microsoft Fabric and Power BI, `pbir` wraps the [Fabric CLI](https://github.com/microsoft/fabric-cli) for convenience. This includes retrieving report or model metadata, publishing reports, and querying semantic models. You are not required to use `pbir` for these operations -- you can use the Fabric CLI directly.

The `pbir` filesystem paradigm (path syntax, item addressing) is inspired by and intentionally designed to be consistent with the Fabric CLI. In our testing, coding agents perform better when tools share familiar conventions. The intent is that the pbir-cli and Fabric CLI can be used interchangeably without confusing agents.

Note that metadata retrieval bypasses sensitivity labels by design, using the `fab get -f` pattern.

For more information, see the [Fabric CLI on GitHub](https://github.com/microsoft/fabric-cli).

## Coding Agents and AI

This project was built with assistance from coding agents and AI tools:

- [Claude Code](https://claude.ai/code) by Anthropic
- [Codex](https://openai.com/index/openai-codex/) by OpenAI
- [Gemini CLI](https://github.com/google-gemini/gemini-cli) by Google
- [GitHub Copilot](https://github.com/features/copilot) by GitHub

## Testers

Thank you to everyone in the community who has tested `pbir`, reported issues, and shared feedback. Your input directly shapes how this tool evolves -- we appreciate the time and effort.
