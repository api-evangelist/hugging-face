---
title: "Designing the hf CLI as an agent-optimized way to work with the Hub"
url: "https://huggingface.co/blog/hf-cli-for-agents"
date: "2026-06-04"
author: "Celina Hanouti"
feed_url: "https://huggingface.co/blog/feed.xml"
---
Hugging Face redesigned its command-line interface to serve both human users and coding agents like Claude Code and Codex. The hf CLI automatically detects agent usage and adjusts output formatting accordingly, rendering rich tables for terminal users while providing compact structured data for agents. Testing revealed that agents using the hf CLI complete complex multi-step Hub tasks with significantly fewer tokens than those relying on curl or the Python SDK, with savings ranging from 1.3x to 6x depending on task complexity.
