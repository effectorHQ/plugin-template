---
name: api-connector
description: "Example skill demonstrating API integration with required CLI tool and environment variables. Shows real-world patterns for skill development."
metadata:
  openclaw:
    emoji: 🔌

    requires:
      bins:
        - curl
      env:
        - API_KEY
        - API_ENDPOINT

    install:
      - id: brew
        kind: brew
        formula: curl
        bins:
          - curl
        label: "Install curl via Homebrew"

      - id: apt
        kind: apt
        package: curl
        bins:
          - curl
        label: "Install curl via APT"

      - id: manual
        kind: manual
        label: "Manual setup"
        steps:
          - "Ensure curl is installed: curl --version"
          - "Set environment variables:"
          - "  export API_KEY='your-api-key-here'"
          - "  export API_ENDPOINT='https://api.example.com'"
          - "  export WORKSPACE_ID='your-workspace-id'"
---

## Purpose

This example demonstrates how to create a real-world OpenClaw skill that:

- Requires external CLI tools (curl)
- Needs environment variables (API_KEY, API_ENDPOINT)
- Calls external APIs
- Provides multiple operations/commands

Use this as a template when building skills that integrate with APIs, cloud services, or other external systems.

## When to Use

- Building API client skills
- Integrating with third-party services
- Creating CLI wrappers for existing tools
- Automating API operations

## When NOT to Use

- For simple operations that don't require external tools
- When the API has an official OpenClaw plugin
- For complex web UI interactions

## Setup

### Prerequisites

1. **curl**: The skill uses curl to make HTTP requests
   - Install with: `brew install curl` (macOS) or `apt install curl` (Linux)
   - Verify: `curl --version`

2. **Environment Variables**: Set these before using the skill
   ```bash
   export API_KEY="sk-1234567890abcdef"
   export API_ENDPOINT="https://api.example.com/v1"
   export WORKSPACE_ID="ws-default"
   ```

3. **Valid API Credentials**: You'll need valid credentials for the API service

### Local Testing

```bash
# Copy the skill to your local directory
cp -r examples/api-connector ~/.openclaw/workspace/skills/api-connector

# Set required environment variables
export API_KEY="test-key-123"
export API_ENDPOINT="https://api.example.com"

# Verify the skill is available
openclaw skills list | grep api-connector

# Test an operation
openclaw invoke api-connector list --resource projects
```

## Commands / Actions

### get

Fetch a resource from the API.

```
openclaw invoke api-connector get --resource <resource> --id <id>
```

Examples:
```bash
openclaw invoke api-connector get --resource users --id user-123
openclaw invoke api-connector get --resource projects --id proj-456
```

### list

List resources of a given type.

```
openclaw invoke api-connector list --resource <resource> [--limit 10]
```

Examples:
```bash
openclaw invoke api-connector list --resource users
openclaw invoke api-connector list --resource projects --limit 25
```

### create

Create a new resource.

```
openclaw invoke api-connector create --resource <resource> --data '<json>'
```

Examples:
```bash
openclaw invoke api-connector create --resource users --data '{"name":"Alice","email":"alice@example.com"}'
```

### delete

Delete a resource.

```
openclaw invoke api-connector delete --resource <resource> --id <id>
```

Examples:
```bash
openclaw invoke api-connector delete --resource users --id user-123
```

## Examples

### List all projects in your workspace

```bash
export API_KEY="sk-prod123"
export API_ENDPOINT="https://api.myservice.com/v1"
openclaw invoke api-connector list --resource projects
```

Output: A JSON list of projects

### Get details about a specific user

```bash
openclaw invoke api-connector get --resource users --id user-789
```

Output: User details including name, email, created_at, etc.

### Create a new project

```bash
openclaw invoke api-connector create \
  --resource projects \
  --data '{"name":"Q1 Planning","owner":"team-a"}'
```

Output: Newly created project with assigned ID

### Delete a resource

```bash
openclaw invoke api-connector delete --resource users --id user-old-456
```

Output: Confirmation of deletion

## Notes

- **API Credentials**: Store `API_KEY` in environment variables, never in scripts or version control
- **Error Handling**: The skill will fail gracefully if the API is unreachable or credentials are invalid
- **Rate Limiting**: Be aware of API rate limits when making bulk requests
- **HTTPS Only**: Always use HTTPS endpoints for security
- **Authentication**: The skill passes your API_KEY in the `Authorization: Bearer` header

### Troubleshooting

**Problem**: `curl: command not found`
- Solution: Install curl with `brew install curl` or `apt install curl`

**Problem**: `401 Unauthorized`
- Solution: Check that API_KEY is set correctly and hasn't expired
- Verify: `echo $API_KEY`

**Problem**: `Connection refused`
- Solution: Verify API_ENDPOINT is correct and the service is running
- Test: `curl -I $API_ENDPOINT`

**Problem**: `WORKSPACE_ID not found`
- Solution: Ensure WORKSPACE_ID environment variable is set
- Set with: `export WORKSPACE_ID="your-workspace-id"`

## Security Considerations

- Never commit credentials to GitHub
- Use environment variables or secure vaults for API keys
- Rotate API keys regularly
- Use HTTPS endpoints only
- Consider IP whitelisting if available
