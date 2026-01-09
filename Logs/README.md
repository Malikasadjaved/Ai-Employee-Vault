# Logs Folder

System logs and audit trails.

## Log Files

### Daily Logs
Format: `YYYY-MM-DD.json`

Each log entry contains:
- timestamp
- action_type
- actor (claude_code, human, etc.)
- target
- parameters
- approval_status
- result

### Log Retention
- Keep daily logs for 90 days
- Monthly summaries kept indefinitely
- Critical errors logged separately

## Example Log Entry
```json
{
  "timestamp": "2026-01-08T10:30:00Z",
  "action_type": "email_draft",
  "actor": "claude_code",
  "target": "client@example.com",
  "parameters": {"subject": "Invoice Request Response"},
  "approval_status": "pending",
  "result": "draft_created"
}
```
