![macOS iMessage MCP Server banner](./assets/banner.svg)

# macOS iMessage MCP Server

[![Platform](https://img.shields.io/badge/platform-macOS-111827?style=for-the-badge&logo=apple)](https://www.apple.com/macos/)
[![Transport](https://img.shields.io/badge/MCP-stdio-34C759?style=for-the-badge)](https://modelcontextprotocol.io/)
[![Messages](https://img.shields.io/badge/messages-local%20access-0B3B2E?style=for-the-badge)](#quickstart)
[![Build](https://img.shields.io/badge/build-npm%20run%20build-1F2937?style=for-the-badge)](#quickstart)

Local MCP server for macOS Messages.

This server gives an MCP client local access to your Messages data on macOS.

It is meant to let an agent:

- search contacts
- resolve names onto chats and message senders
- list recent conversations
- read message history
- search messages
- inspect unread chats
- send iMessages

Under the hood it uses:

- `Contacts.framework` for contact lookup
- `~/Library/Messages/chat.db` for reading chats and messages
- AppleScript for sending through the Messages app

## Quickstart

1. Open the Messages app and make sure you are signed in.
2. Open `System Settings > Privacy & Security > Full Disk Access` and enable it for the app that runs this MCP server.
3. Open `System Settings > Privacy & Security > Contacts` and enable Contacts access for the app that runs this MCP server.
4. If macOS prompts for automation permission when sending, allow the host app to control `Messages`.
5. Fully quit and reopen the host app after changing permissions.
6. Install and build the server:

```bash
npm install
npm run build
```

7. Configure your MCP host to run:

```bash
node /absolute/path/to/build/index.js
```

After that, the MCP client should be able to read chats, resolve contacts, and send iMessages from this Mac.
