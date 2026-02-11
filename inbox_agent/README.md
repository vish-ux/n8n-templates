# AI-Powered Email Inbox Agent

An intelligent n8n workflow that automatically classifies, labels, and responds to incoming Gmail messages using AI. The workflow monitors your inbox, categorizes emails, and takes appropriate actions based on the classification.

## 📋 Overview

This workflow automates email management by:

- Continuously monitoring your Gmail inbox for new messages
- Classifying emails into 4 categories using AI
- Automatically applying Gmail labels for organization
- Generating AI-powered responses for customer support inquiries
- Routing billing emails to your finance team
- Creating drafts for high-priority messages
- Auto-archiving promotional emails

## ✨ Features

- **AI Email Classification**: Uses OpenAI to categorize emails into predefined categories
- **Automated Labeling**: Applies Gmail labels based on email category
- **Smart Customer Support**: AI agent responds to support queries automatically
- **Team Routing**: Forwards billing inquiries to finance team
- **Priority Handling**: Creates draft responses for urgent emails requiring review
- **Spam Management**: Auto-marks promotional emails as read
- **Real-time Processing**: Polls inbox every minute for instant handling

## 🎯 Use Cases

- Managing high-volume customer support inboxes
- Automating first-line email responses
- Organizing inbox with intelligent categorization
- Routing specialized inquiries to appropriate teams
- Reducing email response time
- Filtering and prioritizing urgent messages
- Building AI-powered email assistants

## 🔧 Prerequisites

Before using this workflow, you'll need:

1. **n8n Instance**: Self-hosted or cloud-hosted n8n
2. **Gmail Account**: Business or personal Gmail account
3. **API Credentials**:
   - Gmail OAuth2 (Google Cloud Console)
   - OpenAI API key
4. **Gmail Labels**: Pre-created labels in your Gmail account

## 📊 Email Categories

The workflow classifies emails into 4 categories:

### 1. Customer Support

**Description**: Help requests, troubleshooting, bug reports

- **Keywords**: error, issue, problem, help, access, support, fix, reset, troubleshoot
- **Action**: AI agent generates and sends automatic reply
- **Label**: "Support"

### 2. Finance/Billing

**Description**: Payment, invoice, refund, or subscription inquiries

- **Keywords**: invoice, payment, refund, billing, charge, subscription, renewal, receipt
- **Action**: Forwards to billing team at `billing@example.com`
- **Label**: "Finance"

### 3. High Priority

**Description**: Urgent or time-sensitive matters requiring immediate attention

- **Keywords**: urgent, ASAP, critical, outage, down, immediate, emergency, escalation
- **Action**: Creates draft reply for manual review
- **Label**: "Priority"

### 4. Promotion

**Description**: Marketing emails, newsletters, promotional offers

- **Keywords**: sale, offer, discount, promotion, new feature, upgrade, limited time, exclusive
- **Action**: Marks as read automatically
- **Label**: "Promotion"

## 🚀 Setup Instructions

### 1. Import the Workflow

1. Open n8n
2. Click **Import from File**
3. Select `template.json`
4. Click **Import**

### 2. Create Gmail Labels

Create these labels in your Gmail account:

1. Go to Gmail Settings → Labels
2. Create new labels:
   - `Support`
   - `Finance`
   - `Priority`
   - `Promotion`
3. Note down the Label IDs (or use the workflow to discover them)

### 3. Configure Credentials

#### Gmail OAuth2

1. **Create Google Cloud Project**:
   - Go to [Google Cloud Console](https://console.cloud.google.com)
   - Create new project or select existing one
   - Enable Gmail API

2. **Create OAuth2 Credentials**:
   - Navigate to **APIs & Services** → **Credentials**
   - Create **OAuth 2.0 Client ID**
   - Application type: **Web application**
   - Add authorized redirect URI: `https://your-n8n-instance.com/rest/oauth2-credential/callback`

3. **Configure in n8n**:
   - Open **Gmail Trigger** node
   - Click **Create New Credential**
   - Enter Client ID and Client Secret
   - Authenticate with your Gmail account
   - Grant necessary permissions

4. **Apply to all Gmail nodes**: Use the same credential for all Gmail nodes in the workflow

#### OpenAI API

1. Get API key from [OpenAI Platform](https://platform.openai.com/api-keys)
2. Open **OpenAI Chat Model** node
3. Create new credential with your API key
4. Save and test connection

### 4. Update Gmail Label IDs

Each label operation node needs the correct Label ID:

1. **Get Label IDs**:
   - Temporarily add a Gmail node with "Get All Labels" operation
   - Execute to see all label names and IDs
   - Copy the IDs for your labels

2. **Update Label Nodes**:
   - **Support** node: Replace `Label_1594706753190197855` with your Support label ID
   - **Finance** node: Replace `Label_3029742840171510077` with your Finance label ID
   - **Priority** node: Replace `Label_8750970712842772917` with your Priority label ID
   - **Promotion** node: Replace `Label_1276623808023834907` with your Promotion label ID

### 5. Customize Email Addresses

#### Billing Team Email

1. Open **Send a message** node
2. Update `sendTo` field:
   ```
   billing@example.com → your-finance-team@yourdomain.com
   ```

#### AI Assistant Identity

1. Open **Customer Support Agent** node
2. Customize the system message:
   - Replace "Mani's AI Assistant" with your assistant name
   - Update company name from "AI Automation" to your company
   - Change contact email from `maniaiassistant@example.com`

### 6. Adjust Poll Frequency

By default, the workflow checks for new emails every minute:

1. Open **Gmail Trigger** node
2. Modify poll interval:
   - Every 30 seconds
   - Every 2 minutes
   - Every 5 minutes
3. Balance between responsiveness and API quota usage

## 🎮 How to Use

### Activate the Workflow

1. Ensure all credentials are configured
2. All Gmail label IDs are updated
3. Click **Active** toggle in n8n
4. Workflow will start monitoring your inbox

### Testing

1. **Send Test Emails** to your Gmail account:

   ```
   Support Test: "I'm having trouble logging into my account"
   Billing Test: "When will I receive my invoice?"
   Priority Test: "URGENT: Service is down for all users"
   Promotion Test: "Limited time offer - 50% off!"
   ```

2. **Verify Actions**:
   - Check if labels are applied correctly
   - Verify support emails get automatic replies
   - Confirm billing team receives forwarded email
   - Review draft created for priority emails
   - Check promotional emails are marked as read

### Monitoring

- View execution history in n8n
- Check Gmail for applied labels
- Monitor AI response quality
- Review drafts before sending (Priority emails)

## 🔍 Workflow Architecture

```
Gmail Trigger (Poll every minute)
    ↓
Text Classifier (AI categorization)
    ↓
    ├─→ Customer Support
    │   ↓
    │   Add "Support" Label
    │   ↓
    │   AI Agent (Generate Response)
    │   ↓
    │   Reply to Message
    │
    ├─→ Finance/Billing
    │   ↓
    │   Add "Finance" Label
    │   ↓
    │   Forward to Billing Team
    │
    ├─→ High Priority
    │   ↓
    │   Add "Priority" Label
    │   ↓
    │   Create Draft Reply
    │
    └─→ Promotion
        ↓
        Add "Promotion" Label
        ↓
        Mark as Read
```

## ⚙️ Customization Options

### Add More Categories

1. Open **Text Classifier** node
2. Add new category:
   ```json
   {
     "category": "Partnership",
     "description": "Business partnership or collaboration inquiries..."
   }
   ```
3. Create corresponding Gmail label
4. Add label operation node
5. Define action for this category

### Customize AI Responses

Edit **Customer Support Agent** system message:

- Change tone (formal, casual, friendly)
- Add specific product knowledge
- Include company policies
- Add signature or contact information
- Define escalation criteria

### Change Classification Model

1. Replace **OpenAI Chat Model** with:
   - Google Gemini
   - Anthropic Claude
   - Local LLM
2. Adjust parameters for accuracy vs. cost

### Add Additional Actions

For each category, you can add:

- Slack/Discord notifications
- Database logging
- CRM integration (Salesforce, HubSpot)
- Task creation (Asana, ClickUp)
- Calendar scheduling

## 🐛 Troubleshooting

### Common Issues

**Problem**: Emails not being detected

- **Solution**: Check Gmail OAuth2 permissions; verify trigger is active; check poll frequency

**Problem**: Wrong classification

- **Solution**: Refine category descriptions in Text Classifier; add more keywords; test with diverse samples

**Problem**: Labels not applying

- **Solution**: Verify Label IDs are correct; check Gmail API permissions; ensure labels exist

**Problem**: AI responses are generic

- **Solution**: Enhance system prompt in Customer Support Agent; provide more context; add knowledge base

**Problem**: Hitting API rate limits

- **Solution**: Reduce poll frequency; limit concurrent executions; upgrade API plan

### Gmail API Quotas

Gmail API has usage limits:

- **Free tier**: 250 quota units/user/second
- **Paid tier**: Higher limits available

Actions and their costs:

- Read email: 5 units
- Send email: 100 units
- Modify labels: 5 units

**Optimization tips**:

- Increase poll interval
- Add filters to reduce processed emails
- Batch operations when possible

## 🔒 Security Considerations

- **OAuth2 Scopes**: Only grant minimum required Gmail permissions
- **API Keys**: Store OpenAI key securely in n8n credentials manager
- **Sensitive Data**: Avoid logging email content in plain text
- **Access Control**: Restrict n8n workflow access to authorized users
- **Email Forwarding**: Verify billing team email is correct
- **Draft Review**: Always review AI-generated drafts before sending (Priority category)

## 📈 Performance Optimization

1. **Reduce API Calls**:
   - Add Gmail filters to exclude certain senders
   - Skip already-labeled emails
   - Implement caching for frequent queries

2. **Improve Classification Accuracy**:
   - Collect misclassified examples
   - Refine category descriptions
   - Add more specific keywords
   - Use few-shot examples

3. **Speed Up Processing**:
   - Use faster AI models for classification
   - Parallelize independent operations
   - Optimize prompt length

4. **Cost Management**:
   - Monitor token usage
   - Use cheaper models for simple tasks
   - Implement daily spending limits

## 🧪 Advanced Features

### Add Knowledge Base

Enhance AI responses with RAG (Retrieval-Augmented Generation):

1. Add **Vector Store** node
2. Upload FAQs, documentation, policies
3. Connect to **Customer Support Agent**
4. AI will reference knowledge base in responses

### Multi-Language Support

1. Add language detection in classifier
2. Create language-specific agents
3. Route to appropriate agent based on detected language

### Sentiment Analysis

1. Add sentiment analysis node after classification
2. Prioritize negative sentiment emails
3. Route to human agent if very negative

### Response Templates

1. Create template library for common questions
2. Use embeddings to match incoming email to template
3. Personalize template with AI
4. Faster responses for common queries

## 📊 Analytics & Reporting

Track workflow performance:

- Classification accuracy by category
- Average response time
- Number of emails processed per day
- AI token usage and costs
- Most common support topics

Implement by adding:

- Google Sheets logging
- Database storage (PostgreSQL, MongoDB)
- Analytics dashboard (Grafana, Metabase)

## 🤝 Best Practices

1. **Test thoroughly** before enabling on production inbox
2. **Start with drafts** for all categories, then enable auto-send gradually
3. **Review AI responses** regularly for quality
4. **Maintain escalation path** for complex queries
5. **Update categories** based on actual email patterns
6. **Monitor false positives** and adjust descriptions
7. **Keep system prompts current** with latest product info
8. **Backup important emails** before auto-archiving

## 🔄 Integration Ideas

Extend functionality by connecting with:

- **CRM**: Salesforce, HubSpot - log interactions
- **Help Desk**: Zendesk, Freshdesk - create tickets
- **Communication**: Slack, Teams - send notifications
- **Database**: Store classification results
- **Analytics**: Track response metrics
- **Calendar**: Schedule follow-ups

## 📝 Version History

- **v1.0** - Initial release
  - 4-category email classification
  - AI-powered support responses
  - Automated labeling and routing

## 📄 License

This workflow template is provided as-is for educational and commercial use.

## 🆘 Support

If you encounter issues:

1. Check n8n execution logs for error details
2. Verify Gmail OAuth2 permissions
3. Test OpenAI API connection
4. Confirm Label IDs are correct
5. Review system prompts and category descriptions

## ⚠️ Important Notes

- **AI Responses**: Always review AI-generated responses before enabling auto-send
- **Privacy**: Be mindful of data privacy when processing emails
- **Escalation**: Ensure complex queries can reach human support
- **Testing**: Use a test Gmail account before deploying to production
- **Monitoring**: Regularly check workflow execution for errors

---

Transform your inbox from overwhelming to organized with AI! 📧✨