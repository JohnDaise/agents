---
name: evinced-sdk-setup
description: "Workflow for setting up and managing Evinced web SDK and MCP server. Use when: starting Evinced server, checking Evinced server status, configuring evinced-web-sdk, troubleshooting Evinced MCP tools, running accessibility scans with Evinced."
---

# Evinced SDK Setup and Management

## Purpose
This skill provides step-by-step guidance for setting up, starting, and managing the Evinced web SDK and its MCP (Model Context Protocol) server. Use this skill when you need to configure or troubleshoot the Evinced accessibility testing infrastructure.

## When to Use This Skill
- Before running accessibility scans with Evinced
- When Evinced MCP tools are not available or timing out
- Setting up Evinced for the first time in a project
- Troubleshooting Evinced server connection issues
- Verifying Evinced server is running correctly
- Restarting or stopping the Evinced server

## Evinced Architecture Overview

**Evinced-Latest MCP Server** provides:
- Automated accessibility scanning via browser automation
- WCAG compliance checking
- Detailed issue reporting with DOM snippets
- Remediation guidance for accessibility issues

**Available MCP Tools:**
- `mcp_evinced-lates_evinced_analyze_page` - Scan pages or components for accessibility issues
- `mcp_evinced-lates_evinced_get_issue_details` - Get detailed fix instructions for specific issues

## Workflow: Check and Start Evinced Server

### Step 1: Check if Evinced MCP Tools are Available

First, attempt to search for the Evinced MCP tools to verify the server is running:

```bash
# Search for Evinced tools (this will only work if tools are available)
tool_search_tool_regex pattern="evinced"
```

**Expected Behavior:**
- ✅ **Server Running**: Tools appear in search results (`mcp_evinced-lates_evinced_analyze_page`, etc.)
- ❌ **Server Not Running**: No tools found or timeout

If tools are not available, proceed to Step 2.

### Step 2: Check VS Code MCP Server Status

The Evinced server runs as an MCP server within VS Code. Check the MCP server status:

**Option A: Check VS Code Output Panel**
1. Open Command Palette (Cmd/Ctrl + Shift + P)
2. Type "Output: Show Output Channels"
3. Select "MCP Servers" or "GitHub Copilot"
4. Look for messages about "Evinced-Latest" server

**Option B: Check via Terminal**
```bash
# Check if any Evinced-related processes are running
ps aux | grep -i evinced
```

### Step 3: Start the Evinced-Latest MCP Server

If the server is not running, start it through VS Code:

**Method 1: Via VS Code Settings**
1. Open VS Code Settings (Cmd/Ctrl + ,)
2. Search for "MCP" or "Copilot MCP"
3. Ensure the Evinced-Latest server is configured and enabled
4. Restart VS Code to initialize the server

**Method 2: Manual Server Configuration**
Check if the MCP server configuration exists:

```bash
# Check for MCP server configuration (typical location)
# User-level: ~/Library/Application Support/Code/User/settings.json
# Workspace-level: .vscode/settings.json
cat "$HOME/Library/Application Support/Code/User/settings.json" | grep -A 10 "copilot.mcp"
```

**Method 3: Via Command Palette**
1. Open Command Palette (Cmd/Ctrl + Shift + P)
2. Type "MCP: Manage Servers" or "Copilot: Manage MCP Servers"
3. Find "Evinced-Latest" server
4. Click "Start" or "Restart"

### Step 4: Verify Server is Running

After starting the server, verify it's accessible:

```bash
# Wait a few seconds for server startup, then search for tools
tool_search_tool_regex pattern="evinced"
```

**Success Indicators:**
- ✅ Two Evinced tools appear in search results
- ✅ No timeout or connection errors
- ✅ Tools can be invoked successfully

### Step 5: Test with a Simple Analysis

Once the server is confirmed running, test with a simple page analysis:

```javascript
// Search for and load the analyze_page tool
tool_search_tool_regex pattern="evinced_analyze_page"

// Then invoke it (example)
mcp_evinced-lates_evinced_analyze_page({
  tabMode: "active_tab",
  componentSelector: "",
  model: "claude-sonnet-4"
})
```

## Troubleshooting Common Issues

### Issue: MCP Tools Timeout or Not Found

**Possible Causes:**
1. Server not started
2. Server configuration missing or incorrect
3. VS Code needs restart
4. Insufficient permissions

**Solutions:**
1. Restart VS Code completely
2. Check VS Code MCP server logs (Output panel)
3. Verify server configuration in settings.json
4. Check for error messages in VS Code Developer Tools (Help > Toggle Developer Tools)

### Issue: Server Starts but Tools Don't Work

**Possible Causes:**
1. Browser automation blocked
2. Chrome/Chromium not installed
3. Network/firewall restrictions

**Solutions:**
1. Ensure Chrome or Chromium is installed and accessible
2. Check firewall settings for browser automation
3. Review MCP server logs for specific error messages
4. Try restarting the server

### Issue: Accessibility Scans Fail

**Possible Causes:**
1. Invalid URL provided
2. Page requires authentication
3. Page too slow to load
4. Component selector doesn't match any elements

**Solutions:**
1. Verify URL is accessible and returns 200 status
2. For authenticated pages, ensure you're logged in before scanning
3. Increase timeout if page is slow
4. Test component selector in browser DevTools first

## Best Practices

### Before Starting Work
1. **Always check server status first** before running accessibility scans
2. **Start the server once per session** - it should stay running in background
3. **Verify tools are available** by searching before invoking

### During Development
1. **Keep server running** throughout your work session
2. **Monitor MCP server logs** if experiencing issues
3. **Restart server if tools become unresponsive**

### After Completing Work
1. Server can be left running - VS Code manages lifecycle
2. Server will stop automatically when VS Code closes

## Integration with Other Skills

This skill is a **prerequisite** for:
- **accessibility-expertise** skill - Requires Evinced tools for automated testing
- Any accessibility review or audit work
- WCAG compliance checking

## Quick Reference Commands

```bash
# Check if server is running
tool_search_tool_regex pattern="evinced"

# View process status
ps aux | grep -i evinced

# Check VS Code MCP logs
# Via Command Palette: "Output: Show Output Channels" > "MCP Servers"
```

## Common Workflow Example

```
1. Developer starts accessibility review task
2. AI checks for Evinced MCP tools
3. If not found:
   a. Check VS Code MCP server status
   b. Start Evinced-Latest server via VS Code
   c. Wait for server initialization (10-30 seconds)
   d. Verify tools are now available
4. Proceed with accessibility-expertise skill workflow
5. Keep server running for additional scans
```

## Notes

- The Evinced MCP server is a VS Code extension feature, not a standalone process
- Server management is handled through VS Code's MCP infrastructure
- Tool availability can be checked at any time using `tool_search_tool_regex`
- The server maintains browser automation state across multiple scan requests
- Restart the server if you encounter persistent "browser not responding" errors
