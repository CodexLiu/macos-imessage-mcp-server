# Messages MCP Server

Local MCP server for macOS Messages.

It uses:

- AppleScript for sending iMessages
- `Contacts.framework` through a Swift helper for contact lookup and name resolution
- `~/Library/Messages/chat.db` for inbox and history reads

## Tools

### Core messaging tools

#### `search_contacts`

Find contacts by name, phone number, or email.

Inputs:

- `query`: text to search for

Use this when you need to resolve a person before sending or reading.

#### `send_imessage`

Send an iMessage through the local Messages app.

Inputs:

- `recipient`: phone number or email
- `message`: message text to send

This is the only write tool in the server.

#### `list_chats`

Return recent conversations from the Messages database with resolved participant names.

Inputs:

- `limit`: max number of chats to return
- `query`: optional raw chat search string
- `includeArchived`: whether archived chats should be included

Useful as the inbox view. Output includes:

- `preferredName`
- `participants`
- `messageCount`
- `unreadCount`
- `lastMessageAt`
- `lastMessageText`

#### `read_chat`

Read recent messages from a single conversation.

Inputs:

- `chatId`: numeric chat ID from `list_chats`
- `chatIdentifier`: phone number or email for the chat
- `limit`: max number of messages to return

You can provide either `chatId` or `chatIdentifier`.

#### `get_latest_messages`

Return a global latest-messages feed across all chats.

Inputs:

- `limit`: max number of messages to return

Useful when the model needs the most recent activity without first choosing a chat.

#### `search_messages`

Search recent messages by message text, sender, chat identifier, or chat display name.

Inputs:

- `query`: search text
- `limit`: max number of results

### Higher-level helper tools

#### `get_unread_chats`

Return chats that currently appear to have unread messages.

Inputs:

- `limit`: max number of chats to return

#### `search_chats`

Search chats in a more human way, including contact-name matches resolved through Contacts.

Inputs:

- `query`: search text
- `limit`: max number of chats to return

Use this instead of raw `list_chats` filtering when the query is person-oriented.

#### `get_chat_by_contact`

Find chats associated with a contact search query.

Inputs:

- `query`: person-like search text
- `limit`: max number of chats to return

This is useful when one person may have multiple chat channels such as iMessage, SMS, and RCS.

#### `get_latest_message_for_contact`

Return the latest message from the most recent chat matching a contact query.

Inputs:

- `query`: person-like search text

Use this for prompts like “what was the latest message from Silvia?”

#### `resolve_recipient`

Resolve a person-like query into matching contacts, chats, and suggested recipient identifiers.

Inputs:

- `query`: person-like search text

Useful before sending a message when multiple contacts or channels may match.

### Power-user database tools

#### `get_messages_db_schema`

Inspect the Messages sqlite schema.

Inputs:

- `table`: optional table name

Useful for debugging and future tool development.

#### `query_messages_db`

Run a read-only SQL query directly against the Messages database.

Inputs:

- `sql`: read-only SQL query

Allowed query types are limited to read-only access. This is primarily an admin/debug tool, not the first choice for normal messaging tasks.

## Requirements

- macOS
- Full Disk Access for the host process that reads `~/Library/Messages/chat.db`
- Contacts permission for the Swift helper
- Messages app signed in

## Development

```bash
npm install
npm run build
```

## Architecture

- Contacts are resolved through `scripts/contacts.swift`
- Message and chat history come from `~/Library/Messages/chat.db`
- Sending still goes through AppleScript in the Messages app
