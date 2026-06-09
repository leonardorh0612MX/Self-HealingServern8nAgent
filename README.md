# LeoBunker HomeLab Monitor

An autonomous server monitoring agent built with n8n and Claude AI that automatically detects infrastructure issues, executes fixes via SSH, and reports incidents via Telegram.

## Overview

This workflow runs every 10 minutes, collects server metrics, and triggers an AI agent when something is wrong. The agent analyzes the problem, executes the appropriate fix, and sends a summary to Telegram — no manual intervention required.

## Workflows

### HomeLab Monitor
The main workflow. Collects metrics, detects problems and triggers the AI agent.

**Flow:**
Schedule Trigger (every 10 min)
→ SSH (collect metrics)
→ Code (parse and evaluate metrics)
→ IF problems detected
→ AI Agent (analyze + fix)
Tools: SSH Command Call, Docker Tool
→ Telegram (send incident report)
→ No Operation (if all good)

**Metrics monitored:**
- CPU usage (threshold: > 80%)
- RAM usage (threshold: > 85%)
- Disk usage (threshold: > 90%)
- Battery level (threshold: < 20%)
- HTTP status of leobunker.dev (expected: 200)
- Docker containers running

### SSH - Command Call
A subworkflow used as a tool by the AI agent to execute bash commands on the server via SSH.

## Requirements

- n8n (self-hosted)
- Anthropic API key (Claude Sonnet)
- Telegram Bot token and Chat ID
- SSH access to your Linux server

## Setup

1. Import both workflow JSON files into your n8n instance
2. Configure credentials:
   - **SSH**: add your server credentials under `SSH leobunker_lap1`
   - **Anthropic**: add your API key under `Anthropic account`
   - **Telegram**: add your bot token under `Telegram Leobunker Monitor`
3. Update the Telegram `chatId` in the Send Message node with your own Chat ID
4. Update thresholds in the Code node if needed
5. Activate the HomeLab Monitor workflow

## Server Context

Designed for a Ubuntu 24 LTS server running:
- Ghost blog (Docker, port 3001)
- MySQL (Docker)
- n8n (Docker, port 5678)
- Cloudflare Tunnel

Adjust the system prompt in the AI Agent node to match your own server setup.

## License

MIT License — free to use, modify and distribute.

## Author

Leonardo — [leobunker.dev](https://leobunker.dev)
