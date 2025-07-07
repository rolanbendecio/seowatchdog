# SEO Watchdog - n8n Workflow

Automated SEO monitoring and reporting system that pulls Google Search Console data via BigQuery, analyzes it with Claude AI, and sends insights to Telegram.

## Features

- 📊 **Data Extraction**: Pulls GSC performance data from BigQuery
- 🔍 **Indexing Monitoring**: Tracks newly indexed/deindexed pages
- 🤖 **AI Analysis**: Uses Claude AI to identify SEO issues and opportunities
- 📱 **Telegram Notifications**: Sends formatted reports to Telegram
- ⏰ **Scheduled Reports**: Runs daily at 9 AM (configurable)

## Prerequisites

1. **Google Cloud Platform**:
   - BigQuery API enabled
   - Search Console data exported to BigQuery
   - Service account with BigQuery access

2. **Claude AI API**:
   - Anthropic API key

3. **Telegram Bot**:
   - Bot token from @BotFather
   - Chat ID for the target group/channel

4. **n8n Instance**:
   - Self-hosted or cloud n8n instance

## Setup Instructions

### 1. Environment Variables
Set these in your n8n environment:
```
GCP_PROJECT_ID=your-gcp-project-id
ANTHROPIC_API_KEY=your-claude-api-key
TELEGRAM_BOT_TOKEN=your-bot-token
TELEGRAM_CHAT_ID=your-chat-id
```

### 2. Import Workflow
1. Copy the content of `seo-watchdog-workflow.json`
2. In n8n, go to Workflows > Import from JSON
3. Paste the JSON content

### 3. Configure Credentials
1. **Google BigQuery**: Set up service account authentication
2. **Claude AI**: Add API key in HTTP Request node credentials
3. **Telegram**: Bot token is used in the HTTP Request URL

### 4. Customize BigQuery Queries
The workflow includes two main queries:
- **Performance Data**: Clicks, impressions, CTR, position by page
- **Indexing Data**: Recently indexed/deindexed pages

Modify table names in the queries to match your BigQuery dataset.

## Workflow Components

### 1. Scheduler Node
- Runs daily at 9 AM (cron: `0 9 * * *`)
- Configurable schedule

### 2. BigQuery Nodes
- **GSC Data**: Extracts performance metrics with trend analysis
- **Indexing Data**: Monitors crawl status and indexing changes

### 3. Claude AI Analysis
- Analyzes extracted data for SEO insights
- Identifies traffic drops, CTR issues, and opportunities
- Provides actionable recommendations

### 4. Report Formatting
- Structures insights with emoji indicators
- Formats for Telegram markdown

### 5. Telegram Delivery
- Sends formatted report to specified chat
- Logs delivery status

## Report Sections

The generated report includes:
- 📊 **Summary**: Overall stats
- ⚠️ **Critical Alerts**: Urgent issues
- 🔻 **Traffic Drops**: Pages losing traffic
- 🔼 **Traffic Surges**: Pages gaining traffic
- 📈 **Low CTR Opportunities**: High impression, low CTR pages
- 🎯 **Position Improvement**: Pages ranking 10+
- 🔗 **Indexing Issues**: Deindexed or crawl problems
- ✅ **Recommendations**: Specific actions
- 🚀 **Growth Opportunities**: High-potential pages

## Customization

### Schedule Frequency
Change the cron expression in the scheduler node:
- Daily: `0 9 * * *`
- Weekly: `0 9 * * 1` (Mondays)
- Twice daily: `0 9,17 * * *`

### BigQuery Queries
Modify the SQL queries to:
- Change date ranges
- Add custom filters
- Include additional metrics
- Adjust threshold values

### Claude AI Prompts
Edit the analysis prompt to:
- Focus on specific SEO aspects
- Change alert thresholds
- Customize report format
- Add brand-specific insights

## Troubleshooting

### Common Issues
1. **BigQuery Access**: Ensure service account has proper permissions
2. **Claude API Limits**: Monitor API usage and rate limits
3. **Telegram Delivery**: Verify bot token and chat ID
4. **Data Availability**: Check if GSC data is properly exported to BigQuery

### Monitoring
- Check n8n execution logs
- Monitor BigQuery query costs
- Track Telegram message delivery

## Security Notes
- Store all credentials securely in n8n
- Use service accounts with minimal required permissions
- Regularly rotate API keys
- Monitor for unauthorized access

## Support
For issues or questions, check the n8n community forums or create an issue in this repository.