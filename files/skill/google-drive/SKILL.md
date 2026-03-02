---
name: google-drive
description: Google Drive MCP tools for Drive, Docs, Sheets, Slides, and Calendar operations
---

# Google Drive MCP Integration

**Rule:** For ALL Google Workspace operations (Drive, Docs, Sheets, Slides, Calendar), use the Google Drive MCP server tools.

## Core Principles

1. **MCP tools are available** - The Google Drive MCP server provides direct access to Google Workspace services
2. **Authentication is handled** - OAuth 2.0 handles auth via browser flow after initial setup
3. **Use the right tool** for each operation (see tool reference below)
4. **File IDs are persistent** - Google uses unique file IDs that remain constant even when files are renamed or moved

## Available Tools

### Google Drive Tools
| Tool | Description |
|------|-------------|
| `search` | Search for files and folders in Drive |
| `listFilesInFolder` | List contents of a specific folder |
| `createFolder` | Create a new folder |
| `uploadFile` | Upload a file to Drive |
| `downloadFile` | Download a file from Drive |
| `deleteFile` | Delete a file or folder |
| `renameFile` | Rename a file or folder |
| `moveFile` | Move a file to a different folder |
| `copyFile` | Create a copy of a file |
| `shareFile` | Share a file with users or make public |
| `getFileRevisions` | Get version history of a file |
| `restoreFileRevision` | Restore a previous version |

### Google Docs Tools
| Tool | Description |
|------|-------------|
| `createGoogleDoc` | Create a new Google Doc |
| `readGoogleDoc` | Read content from a Google Doc |
| `updateGoogleDoc` | Update/replace content in a Google Doc |
| `insertTextToGoogleDoc` | Insert text at a specific position |
| `deleteTextFromGoogleDoc` | Delete text from a document |
| `formatTextInGoogleDoc` | Apply formatting (bold, italic, etc.) |
| `addCommentToGoogleDoc` | Add a comment to a document |
| `listGoogleDocComments` | List all comments on a document |

### Google Sheets Tools
| Tool | Description |
|------|-------------|
| `createGoogleSheet` | Create a new spreadsheet |
| `getGoogleSheetContent` | Read data from a sheet |
| `updateGoogleSheetCells` | Write data to cells |
| `addSheetToSpreadsheet` | Add a new sheet/tab |
| `formatGoogleSheetCells` | Apply cell formatting |
| `addConditionalFormatting` | Add conditional formatting rules |

### Google Slides Tools
| Tool | Description |
|------|-------------|
| `createGoogleSlides` | Create a new presentation |
| `updateGoogleSlides` | Update presentation content |
| `addTextBoxToSlide` | Add a text box to a slide |
| `addShapeToSlide` | Add shapes to a slide |
| `getSlideThumbnail` | Get thumbnail image of a slide |

### Google Calendar Tools
| Tool | Description |
|------|-------------|
| `listCalendars` | List available calendars |
| `createCalendarEvent` | Create a new event |
| `getCalendarEvent` | Get event details |
| `updateCalendarEvent` | Update an existing event |
| `deleteCalendarEvent` | Delete an event |

## Common Operations

### Search for Files
```
Use search with query: "name contains 'report' and mimeType = 'application/vnd.google-apps.document'"
```

### List Files in a Folder
```
Use listFilesInFolder with:
- folderId: The folder ID (or 'root' for root folder)
```

### Create a New Document
```
Use createGoogleDoc with:
- title: Document title
- content: Initial content (optional)
- folderId: Parent folder ID (optional)
```

### Read Document Content
```
Use readGoogleDoc with:
- documentId: The document ID from the URL
```

### Update Spreadsheet Cells
```
Use updateGoogleSheetCells with:
- spreadsheetId: The spreadsheet ID
- range: Cell range (e.g., "Sheet1!A1:B10")
- values: 2D array of values
```

### Create Calendar Event
```
Use createCalendarEvent with:
- calendarId: Calendar ID (or 'primary')
- summary: Event title
- start: Start datetime (ISO format)
- end: End datetime (ISO format)
- attendees: List of email addresses (optional)
```

### Share a File
```
Use shareFile with:
- fileId: The file ID
- email: User email to share with
- role: 'reader', 'writer', or 'commenter'
```

## Search Query Reference

```
# Files by name
name contains 'quarterly'

# Documents only
mimeType = 'application/vnd.google-apps.document'

# Spreadsheets only
mimeType = 'application/vnd.google-apps.spreadsheet'

# Presentations only
mimeType = 'application/vnd.google-apps.presentation'

# Files in a specific folder
'FOLDER_ID' in parents

# Files owned by me
'me' in owners

# Recently modified
modifiedTime > '2024-01-01T00:00:00'

# Shared with me
sharedWithMe = true

# Starred files
starred = true

# Combine conditions
name contains 'report' and mimeType = 'application/vnd.google-apps.document' and modifiedTime > '2024-01-01'
```

## MIME Type Reference

| Type | MIME Type |
|------|-----------|
| Google Doc | `application/vnd.google-apps.document` |
| Google Sheet | `application/vnd.google-apps.spreadsheet` |
| Google Slides | `application/vnd.google-apps.presentation` |
| Google Form | `application/vnd.google-apps.form` |
| Folder | `application/vnd.google-apps.folder` |
| PDF | `application/pdf` |
| Word Doc | `application/vnd.openxmlformats-officedocument.wordprocessingml.document` |
| Excel | `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet` |

## OAuth Setup Instructions

### 1. Create a GCP Project
1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a new project or select an existing one

### 2. Enable Required APIs
1. Go to **APIs & Services > Library**
2. Search for and enable each of these APIs:
   - Google Drive API
   - Google Docs API
   - Google Sheets API
   - Google Slides API
   - Google Calendar API

### 3. Configure OAuth Consent Screen
1. Go to **APIs & Services > OAuth consent screen**
2. Select **External** user type (or Internal if using Google Workspace)
3. Fill in the app information:
   - App name: "OpenCode Google Drive MCP"
   - User support email: Your email
   - Developer contact: Your email
4. Add scopes:
   - `https://www.googleapis.com/auth/drive`
   - `https://www.googleapis.com/auth/documents`
   - `https://www.googleapis.com/auth/spreadsheets`
   - `https://www.googleapis.com/auth/presentations`
   - `https://www.googleapis.com/auth/calendar`
5. Add test users (your email address)
6. Save and continue

### 4. Create OAuth Credentials
1. Go to **APIs & Services > Credentials**
2. Click **Create Credentials > OAuth client ID**
3. Select **Desktop app** as the application type
4. Name it "OpenCode MCP Client"
5. Click **Create**
6. Download the JSON file

### 5. Install Credentials
```bash
# Create the config directory
mkdir -p ~/.config/google-drive-mcp

# Move the downloaded JSON file
mv ~/Downloads/client_secret_*.json ~/.config/google-drive-mcp/gcp-oauth.keys.json
```

Alternatively, set the `GOOGLE_OAUTH_CREDS` environment variable to point to your credentials file.

### 6. First Startup (Automatic Authentication)
On the **first** OpenCode startup with the Google Drive MCP enabled:
1. Your browser will automatically open to Google's OAuth consent screen
2. Sign in with your Google account and grant the requested permissions
3. After completing consent, tokens are saved locally to `~/.config/google-drive-mcp/tokens.json`

**No separate auth command is required** - authentication happens automatically when OpenCode initializes the MCP server.

### 7. Subsequent Startups
After the initial authentication, subsequent OpenCode startups are seamless - the saved tokens are reused automatically.

## DO THIS / NOT THIS

### Searching Files
```
# DO THIS - use the MCP tool
search with query: "name contains 'report'"

# NOT THIS - manual API calls
curl -H "Authorization: Bearer ..." https://www.googleapis.com/drive/v3/files
```

### Reading Documents
```
# DO THIS - use the MCP tool
readGoogleDoc with documentId

# NOT THIS - manual API calls or external scripts
```

### Managing Calendar
```
# DO THIS - use the MCP tool
createCalendarEvent with summary, start, end

# NOT THIS - external calendar integrations
```

### Working with File IDs
```
# DO THIS - extract file ID from URL
URL: https://docs.google.com/document/d/1abc123xyz/edit
File ID: 1abc123xyz

# NOT THIS - using full URLs as file identifiers
```

## Troubleshooting

### "Token has been expired or revoked"
**Cause:** OAuth tokens expire after a period of inactivity or if revoked.

**Solution:**
1. Delete the stored tokens:
   ```bash
   rm ~/.config/google-drive-mcp/tokens.json
   ```
2. Restart OpenCode - browser will open automatically for re-authentication

### "invalid_grant" Error
**Cause:** Refresh token is invalid, often due to:
- Test user was removed from OAuth consent screen
- OAuth credentials were regenerated
- Token file is corrupted

**Solution:**
1. Delete the stored tokens:
   ```bash
   rm ~/.config/google-drive-mcp/tokens.json
   ```
2. Restart OpenCode - browser will open automatically for re-authentication

### "Access Not Configured" or "API not enabled"
**Cause:** Required API is not enabled in GCP project.

**Solution:**
1. Go to GCP Console > APIs & Services > Library
2. Search for the specific API (Drive, Docs, Sheets, Slides, Calendar)
3. Click Enable

### "Insufficient Permission" or Scope Issues
**Cause:** OAuth consent screen doesn't include required scopes.

**Solution:**
1. Go to GCP Console > APIs & Services > OAuth consent screen
2. Edit the app and add missing scopes
3. Delete tokens (`rm ~/.config/google-drive-mcp/tokens.json`) and restart OpenCode

### "Access blocked: App not verified"
**Cause:** App is in testing mode and user is not a test user.

**Solution:**
1. Go to GCP Console > APIs & Services > OAuth consent screen
2. Add your email to "Test users"
3. Or publish the app (requires Google verification)

### Tool Not Found
**Cause:** MCP server not properly configured.

**Solution:**
1. Ensure `google_drive` MCP is configured in opencode.jsonc
2. Verify the command path is correct
3. Restart OpenCode after config changes

### File Not Found
**Cause:** Invalid file ID or no access to the file.

**Solution:**
1. Verify the file ID is correct (extract from URL)
2. Check that you have access to the file
3. Ensure the file hasn't been deleted or moved to trash
