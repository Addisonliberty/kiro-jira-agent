# Sprint Ticket Agent

A Kiro custom agent that turns natural-language (or image-based) descriptions of work into properly structured Jira Epics and Stories in the **ECM** project. It always previews a ticket and waits for your explicit approval before writing to Jira.

## 1. Connect to the Atlassian Jira MCP server in Kiro

The agent talks to Jira through the `atlassian-jira-dc` MCP server. You need this configured and authenticated before the agent can read or create tickets.

1. Open your MCP config:
   - User (global): `~/.kiro/settings/mcp.json`
   - or Workspace: `.kiro/settings/mcp.json`
2. Add the Jira Data Center server:

   ```json
   {
     "mcpServers": {
       "atlassian-jira-dc": {
         "command": "npx",
         "args": ["-y", "@atlassian-dc-mcp/jira"],
         "env": {
           "JIRA_HOST": "https://your-jira-host",
           "JIRA_API_TOKEN": "${JIRA_API_TOKEN}"
         },
         "disabled": false,
         "autoApprove": [
           "jira_searchIssues",
           "jira_getIssue",
           "jira_createIssue",
           "jira_updateIssue"
         ]
       }
     }
   }
   ```

   - `JIRA_HOST` — your Jira Data Center base URL.
   - `JIRA_API_TOKEN` — a Jira personal access token (create one in Jira under *Profile → Personal Access Tokens*). The `${JIRA_API_TOKEN}` syntax reads it from an environment variable so the token stays out of the config file.
   - Requires Node.js (for `npx`).

3. Reconnect the server from the **MCP Server** view in the Kiro feature panel (or it reconnects automatically when the config is saved).
4. Verify it's connected — the `atlassian-jira-dc` tools should appear as available.

## 2. Point the session memory agent at your sessions folder

The `session-memory-agent` indexes your past Kiro sessions into a searchable knowledge base so it can recall prior work. It reads them from a folder you need to set for your own machine.

1. Open `.kiro/agents/session-memory-agent.json`.
2. Find the `resources` → `source` field. It ships as:

   ```json
   "source": "file://~/.kiro/sessions/files",
   ```

3. Update it to the absolute path of your own Kiro sessions folder. For example:
   - macOS / Linux: `file:///home/<you>/.kiro/sessions/files`
   - Windows: `file://C:/Users/<you>/.kiro/sessions/files`

   Use forward slashes even on Windows, and point it at the `sessions/files` directory under your user's `.kiro` folder.
4. Save the file. The knowledge base re-indexes automatically (`autoUpdate` is enabled).

## 3. Use the agent

1. In the Kiro CLI, run `/agent` and select **sprint-ticket-agent**.
2. Describe the work in plain language. Examples:
   - "Create a story to add retry logic to the ingest pipeline."
   - "Create an epic 'Search revamp' with stories for indexing, ranking, and the results UI."
   - Attach an image (whiteboard, roadmap, sticky-note board) to draft a full Epic/Story tree from it.
3. The agent prompts for any required fields it needs:
   - **CSP** (None / AWS / Azure / Google / Oracle)
   - **Description**
   - **Epic Link** (auto-linked if an epic was created in the same session)
4. Review the full preview it shows.
5. Approve by saying **"approve"**, **"yes"**, or **"create it"**. Only then does it create the ticket(s) and return the key and link.

### Notes
- All tickets are created in the **ECM** project. Priority defaults to **Low**.
- The agent never writes to Jira without your explicit approval.
- One ticket is created per distinct unit of work; multiple items are created sequentially so keys match order.
