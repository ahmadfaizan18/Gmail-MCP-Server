# Gmail MCP Server

An MCP server that connects AI assistants to Gmail, enabling email operations through natural-language instructions.

Built with Node.js, the Model Context Protocol (MCP) SDK, the Gmail API, Nodemailer, and OAuth 2.0.

## Features

### Email Sending
- Send emails with custom recipients, subjects, and message bodies.
- Supports plain-text and HTML email content.
- Send introduction emails using a predefined format.
- Reuse email templates with Mustache variable substitution.

### Gmail Integration
- Validate Gmail configuration.
- List Gmail labels.
- Retrieve unread emails with optional Gmail search queries.
- Fetch individual emails by message ID.
- Archive emails by removing the `INBOX` label.
- Move emails to the Gmail trash.

### Authentication
- Gmail App Password authentication for sending emails.
- OAuth 2.0 authentication for Gmail API operations.

**Note:** Reading, listing, archiving, and trashing emails require OAuth 2.0 configuration.

## Tech Stack

- **Runtime:** Node.js
- **Language:** JavaScript (ES modules)
- **AI Integration:** Model Context Protocol (MCP)
- **Email Sending:** Nodemailer
- **Gmail API:** Google APIs Node.js client
- **Authentication:** OAuth 2.0 or Gmail App Password
- **Templates:** Mustache
- **Configuration:** dotenv

## Prerequisites

- Node.js and npm
- A Gmail account
- An MCP-compatible client, such as Claude Desktop
- Gmail App Password credentials or Google Cloud OAuth 2.0 credentials, depending on the operations you intend to use

## Installation

Clone your repository and install the dependencies:

```bash
git clone https://github.com/ahmadfaizan18/Gmail-MCP-Server.git
cd Gmail-MCP-Server
npm install
```

## Configuration

Create a `.env` file in the project root.

### Option A: Gmail App Password

This option supports email sending.

1. Enable 2-Step Verification in your Google Account.
2. Generate an App Password if your account is eligible.
3. Add your Gmail address and App Password to `.env`.

```env
GMAIL_USER=your_email@gmail.com
GMAIL_AUTH_METHOD=app_password
GMAIL_APP_PASSWORD=your_app_password
```

Keep your credentials private. Never commit your `.env` file to GitHub.

### Option B: OAuth 2.0

OAuth 2.0 is required for Gmail API operations such as listing unread emails, retrieving messages, archiving, and trashing messages.

1. Create a project in Google Cloud Console.
2. Enable the Gmail API.
3. Configure OAuth consent and create OAuth 2.0 credentials.
4. Obtain a refresh token for the Google account you intend to access.
5. Add the credentials to `.env`.

```env
GMAIL_CLIENT_ID=your_client_id
GMAIL_CLIENT_SECRET=your_client_secret
GMAIL_REDIRECT_URI=your_redirect_uri
GMAIL_REFRESH_TOKEN=your_refresh_token
```

Use the redirect URI configured for your OAuth application. Do not publish your client secret or refresh token.

## Running the Server

Start the server:

```bash
npm start
```

The server communicates over standard input and output using the MCP protocol. It is intended to be launched by an MCP client.

## Claude Desktop Configuration

Add an entry for the server to your Claude Desktop MCP configuration.

```json
{
  "mcpServers": {
    "gmail": {
      "command": "node",
      "args": [
        "/absolute/path/to/Gmail-MCP-Server/index.js"
      ],
      "cwd": "/absolute/path/to/Gmail-MCP-Server"
    }
  }
}
```

Replace the example paths with the absolute path to your local project directory. Restart Claude Desktop after updating its configuration.

## Available Tools

The server exposes the following MCP tools:

| Tool | Description |
|---|---|
| `send_email` | Send an email with a recipient, subject, and body. |
| `send_introduction_email` | Send a formatted introduction email. |
| `check_gmail_config` | Check the Gmail sending configuration. |
| `list_labels` | Retrieve Gmail labels using OAuth 2.0. |
| `list_unread_emails` | Retrieve unread messages, optionally filtered by a Gmail search query. |
| `get_email` | Retrieve an email by message ID. |
| `archive_email` | Remove an email from the inbox. |
| `delete_email` | Move an email to the trash. |
| `list_email_templates` | List available email templates. |
| `send_template_email` | Send an email using a template and variables. |

## Usage Examples

Once configured in your MCP client, you can issue requests such as:

- "Send an email to someone@example.com with subject Hello and body Test message."
- "Check whether my Gmail configuration is working."
- "List my unread emails."
- "List unread emails from a recruiter."
- "Retrieve an email using its message ID."
- "Archive an email using its message ID."
- "Move an email to the trash using its message ID."
- "List available email templates."
- "Send an introduction email using the introduction template."

## Email Templates

Email templates are stored in the `templates/` directory as `.txt` files.

A template may include a `Subject:` line and Mustache placeholders such as `{{recipientName}}`. The server substitutes these placeholders using the variables supplied to the `send_template_email` tool.

For example, a template can use:

```text
Subject: Following up on {{topic}}

Hi {{recipientName}},

I wanted to follow up on {{topic}}.

Best regards,
{{name}}
```

## Security Notes
- Keep `.env` out of version control.
- Never publish Gmail App Passwords, OAuth client secrets, or refresh tokens.
- Use OAuth credentials only for accounts you are authorized to access.
- Review the permissions granted to your Google OAuth application.
- Avoid logging sensitive email contents or authentication credentials.

## License
See the repository's license file for licensing terms.