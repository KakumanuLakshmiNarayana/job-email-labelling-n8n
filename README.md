# 📬 Job Email Labelling Automation (n8n Workflow)

This n8n workflow automatically classifies incoming job-related emails in your Gmail inbox into categories such as:
- ✅ Applications
- 📅 Interviews
- 📢 Job Alerts
- 🔗 LinkedIn Short Applications
- ❌ Rejections
- 📂 Others

Emails are automatically labeled and categorized based on their content using a custom-trained AI classifier powered by OpenAI.

---

## 🧠 How It Works

1. **Gmail Trigger**: Polls Gmail every minute for new emails.
2. **Text Classifier**: Uses a custom prompt with category rules to classify emails using OpenAI's GPT model (`gpt-4o-mini`).
3. **Gmail Labeler**: Applies appropriate Gmail labels based on the classified category.
4. **Calendar Sync (optional)**: If the classified category is "Interview", it optionally attempts to check or sync with Google Calendar.

---

## 🛠 Setup Instructions

### 1. Import the Workflow

- In your n8n instance, go to `Workflows` → `Import` → upload the `kln0586_email_labelling.json` file.

### 2. Configure Credentials

The following credentials need to be configured in **n8n's credential manager** (not stored in the workflow):

| Credential Name           | Service             | Notes |
|---------------------------|---------------------|-------|
| `Gmail account 2`         | Gmail OAuth2        | Used by trigger and label nodes |
| `OpenAi account`          | OpenAI API Key      | Used by text classifier node |
| `Google Calendar account` | Google Calendar API | Optional, used to detect calendar events for interviews |

> ⚠️ **Important:** Never commit actual credential details into Git. This workflow only includes references (names) to those credentials.

### 3. Create Gmail Labels

Ensure the following labels exist in your Gmail account and note their internal label IDs:

- Applications
- Interview
- Job Alerts
- LinkedIn Short Applications
- Rejections
- Others

If you don’t know the label ID, you can find them by querying Gmail labels via the [Gmail API](https://developers.google.com/gmail/api/guides/labels) or temporarily using an n8n node to log them.

### 4. Optional: Enable Google Calendar Check

This node is connected to the "Interview" label path to cross-reference event timing using Gmail email metadata (`internalDate`). It's currently optional and only enabled if configured.

---

## 🔐 Security Notes

- This workflow **does not contain any secrets or tokens**.
- All credentials are referenced by name and securely managed inside n8n.
- Email addresses and personal data have been stripped before sharing.

---

## 📎 Example Classification Prompts

Each category is defined by strict rules. For example:

- **Applications**: Looks for phrases like _"Your application has been received"_, _"Application submitted"_.
- **LinkedIn Applications**: Must explicitly mention application via LinkedIn.
- **Rejections**: Includes phrases like _"We regret to inform you"_, _"Not moving forward"_.
- **Others**: Catches all unmatched or ambiguous messages.

---

## 📂 File Contents

```bash
.
├── kln0586_email_labelling.json    # Main n8n workflow file
└── README.md                       # Workflow documentation
```

---

## 📧 Author

Created by [Your Name or GitHub Handle]  
Contributions and suggestions are welcome!
