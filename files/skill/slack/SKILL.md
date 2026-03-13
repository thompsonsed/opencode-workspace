---
name: slack
description: Slack MCP tools for workspace communication
---

# Slack MCP Integration

The Slack MCP server provides tools for reading and posting messages, managing reactions, and accessing user information in your Slack workspace. Use these tools for all Slack operations instead of direct API calls.

## Setup

Authentication is handled automatically via browser OAuth when you first use a Slack tool.

1. Use any Slack tool (e.g., "list my Slack channels")
2. A browser window will open for Slack OAuth
3. Authorize the app to access your workspace
4. Authentication persists for future sessions

## Available Tools

| Tool | Description |
|------|-------------|
| `slack_list_channels` | List public channels in workspace |
| `slack_post_message` | Post message to a channel |
| `slack_reply_to_thread` | Reply to a message thread |
| `slack_add_reaction` | Add emoji reaction to message |
| `slack_get_channel_history` | Get recent messages from channel |
| `slack_get_thread_replies` | Get replies in a thread |
| `slack_get_users` | List workspace users |
| `slack_get_user_profile` | Get detailed user profile |

## Key Notes

- Channel IDs start with `C` (e.g., `C01234567`)
- Thread timestamps are message identifiers (e.g., `1234567890.123456`)
- Bot must be invited to channels to read/post messages

## Disable/Rollback

Set `"enabled": false` in the slack MCP config in opencode.jsonc
