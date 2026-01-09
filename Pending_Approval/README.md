# Pending Approval Folder

This folder contains actions that require human approval before execution.

## How It Works

1. **AI Creates Request**: When Claude identifies an action requiring approval, it creates a file here
2. **Human Reviews**: You review the details in the file
3. **Approve**: Move file to /Approved folder to proceed
4. **Reject**: Move file to /Rejected folder to cancel

## Approval Required For
- Financial transactions over $50
- Emails to new contacts
- Payments to new recipients
- Any irreversible action
- Sensitive communications

## File Naming Convention
`ACTION_TYPE_Description_YYYY-MM-DD.md`

Examples:
- `PAYMENT_Client_A_2026-01-08.md`
- `EMAIL_New_Contact_2026-01-08.md`
