# SEO Watchdog - Complete Setup Guide

## 🎯 Overview

This guide will help you set up the complete SEO Watchdog workflow that includes:
- ✅ Batch processing for large datasets (prevents timeouts)
- ✅ Keyword prioritization with scoring system
- ✅ Google Sheets integration for tracked keywords
- ✅ Performance analysis with tracked keyword awareness
- ✅ Indexing and crawl status monitoring
- ✅ AI-powered analysis with Claude
- ✅ Automated Telegram reporting

## 📋 Prerequisites

### 1. Google Cloud Platform Setup
- Google Cloud Platform account
- BigQuery API enabled
- Google Search Console data exported to BigQuery
- Service account with BigQuery access

### 2. Google Sheets Setup
- Google account with Sheets access
- Service account with Sheets API access

### 3. Claude AI Setup
- Anthropic Claude API account
- API key for Claude access

### 4. Telegram Setup
- Telegram bot token
- Telegram chat/channel ID

### 5. n8n Workflow Platform
- n8n instance (cloud or self-hosted)
- Proper credentials configuration

## 🔧 Environment Variables

Set these environment variables in your n8n instance:

```bash
# BigQuery Configuration
GCP_PROJECT_ID=your-gcp-project-id

# Google Sheets Configuration
TRACKED_KEYWORDS_SHEET_ID=your-tracked-keywords-sheet-id
KEYWORD_RESULTS_SHEET_ID=your-results-sheet-id

# Telegram Configuration
TELEGRAM_BOT_TOKEN=your-telegram-bot-token
TELEGRAM_CHAT_ID=your-telegram-chat-id
```

## 📊 Google Sheets Setup

### 1. Tracked Keywords Sheet

Create a Google Sheet with ID `TRACKED_KEYWORDS_SHEET_ID` and add a worksheet named "Keywords" with these columns:

| Column A | Column B | Column C | Column D | Column E |
|----------|----------|----------|----------|----------|
| Keyword | Priority | Target Position | Business Value | Notes |
| seo tools | High | 3 | 9 | Main product keyword |
| keyword research | Medium | 5 | 7 | Supporting content |
| seo analysis | Low | 10 | 5 | General tracking |

**Column Descriptions:**
- **Keyword** (A): The exact keyword to track (required)
- **Priority** (B): High/Medium/Low manual priority
- **Target Position** (C): Target ranking position (1-20)
- **Business Value** (D): Business importance score (1-10)
- **Notes** (E): Any additional notes or context

### 2. Results Sheet

Create a Google Sheet with ID `KEYWORD_RESULTS_SHEET_ID` for storing analysis results. The workflow will automatically create the "Analysis Results" worksheet with these columns:

- Date, Keyword, Page, Clicks, Impressions, CTR, Position
- Priority Score, Enhanced Score, Priority, Opportunity, Health
- Tracked, Manual Priority, Business Value, Meets Target

## 🔑 Credentials Setup

### 1. Google Cloud Service Account

1. Create a service account in Google Cloud Console
2. Download the JSON key file
3. Grant these permissions:
   - BigQuery Data Viewer
   - BigQuery Job User
   - Google Sheets API access

4. In n8n, create a credential of type "Google Service Account" with the JSON key

### 2. Claude AI Credentials

1. Get your Claude API key from Anthropic
2. In n8n, create a credential of type "HTTP Header Auth"
3. Set header name: `x-api-key`
4. Set header value: `your-claude-api-key`

### 3. Telegram Bot

1. Create a bot via @BotFather on Telegram
2. Get the bot token
3. Add the bot to your chat/channel
4. Get the chat ID using the Telegram Bot API

## 📂 BigQuery Tables Required

Ensure your BigQuery project has these tables from Google Search Console:

```sql
-- Performance data table
`your-project.searchconsole.searchdata_site_impression`

-- Crawl stats table (optional, for indexing analysis)
`your-project.searchconsole.crawl_stats`
```

**Table Structure for `searchdata_site_impression`:**
- date (DATE)
- page (STRING)
- query (STRING)
- device (STRING)
- clicks (INTEGER)
- impressions (INTEGER)
- position (FLOAT)

## 🎯 Workflow Configuration

### 1. Import the Workflow

1. Copy the content from `seo-watchdog-complete.json`
2. Import into your n8n instance
3. Configure credentials for each node

### 2. Configure Batch Sizes

In the "Master Configuration" node, adjust these settings based on your data volume:

```javascript
const masterConfig = {
  batch_settings: {
    performance_batch_size: 100,    // Reduce if you have timeout issues
    keywords_batch_size: 50,        // Adjust based on tracked keywords
    indexing_batch_size: 100,       // Adjust for indexing data volume
    tracked_keywords_limit: 200     // Maximum keywords to track
  }
}
```

### 3. Priority Scoring Weights

Customize the priority scoring in "Master Configuration":

```javascript
priority_scoring: {
  weights: {
    impressions_weight: 0.3,    // 30% - Search volume importance
    position_weight: 0.25,      // 25% - Current ranking importance
    ctr_weight: 0.2,           // 20% - Click-through rate importance
    clicks_weight: 0.15,       // 15% - Actual traffic importance
    trend_weight: 0.1          // 10% - Performance trend importance
  }
}
```

## 🚀 Testing the Workflow

### 1. Test Individual Nodes

1. Start with "Load Tracked Keywords" node
2. Verify Google Sheets connection
3. Test BigQuery connections
4. Validate Claude AI access
5. Check Telegram delivery

### 2. Full Workflow Test

1. Disable the cron trigger initially
2. Use manual trigger for testing
3. Check all node outputs
4. Verify data quality
5. Confirm report delivery

### 3. Monitor Execution

1. Check n8n execution logs
2. Monitor BigQuery query performance
3. Verify Google Sheets updates
4. Confirm Telegram notifications

## 🔧 Troubleshooting

### Common Issues

#### 1. BigQuery Timeouts
**Symptoms:** Workflow fails at BigQuery nodes
**Solution:** 
- Reduce batch sizes in Master Configuration
- Add date range filters
- Optimize queries for your data volume

#### 2. Google Sheets Access Errors
**Symptoms:** Cannot read tracked keywords
**Solution:**
- Verify service account permissions
- Check sheet ID environment variables
- Ensure proper worksheet names

#### 3. Claude AI Rate Limits
**Symptoms:** AI analysis fails
**Solution:**
- Add delays between requests
- Reduce token usage
- Check API quota limits

#### 4. Memory Issues
**Symptoms:** Workflow crashes with large datasets
**Solution:**
- Reduce batch sizes
- Implement data pagination
- Optimize node memory usage

### Performance Optimization

#### 1. BigQuery Optimization
```sql
-- Add these optimizations to queries:
-- 1. Partition filtering
WHERE date >= DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)

-- 2. Early filtering
AND impressions >= 5

-- 3. Row limits
LIMIT 100
```

#### 2. Workflow Optimization
- Use parallel processing where possible
- Implement error handling
- Add retry logic for failed requests
- Monitor execution time

## 📊 Expected Results

### Daily Reports Include:

1. **Executive Dashboard**
   - Keywords analyzed count
   - Pages monitored count
   - Critical issues summary
   - Overall health status

2. **Critical Actions**
   - Urgent issues requiring immediate attention
   - Timeline for fixes
   - Expected impact

3. **Strategic Opportunities**
   - Quick wins (7 days)
   - Strategic wins (30 days)
   - Long-term goals (quarterly)

4. **Technical Health**
   - Indexing status
   - Crawl issues
   - Performance problems

### Data Storage:
- Historical keyword performance
- Priority trend analysis
- Business value tracking
- Target achievement monitoring

## 🔄 Maintenance

### Weekly Tasks:
- Review tracked keywords list
- Update business values
- Adjust priority settings
- Monitor workflow performance

### Monthly Tasks:
- Analyze historical trends
- Update target positions
- Review priority scoring weights
- Optimize batch sizes

### Quarterly Tasks:
- Strategic keyword review
- Workflow performance audit
- Feature enhancement planning
- ROI analysis

## 📈 Success Metrics

Track these KPIs to measure success:

### Keyword Performance:
- Position improvements
- CTR increases
- Traffic growth
- Target achievement rate

### Technical Health:
- Indexing issue resolution
- Crawl error reduction
- Page performance improvements

### Workflow Efficiency:
- Execution success rate
- Processing time optimization
- Error rate reduction
- Data quality improvement

## 🆘 Support

For additional help:

1. **Check n8n documentation** for workflow troubleshooting
2. **Review BigQuery logs** for query performance issues
3. **Monitor Google Sheets API quotas** for access limits
4. **Check Claude AI status** for service availability
5. **Verify Telegram bot permissions** for delivery issues

## 📝 Version History

- **v3.0**: Complete unified workflow with all features
- **v2.2**: Added keyword prioritization
- **v2.1**: Implemented batch processing
- **v2.0**: Enhanced with comments and instructions
- **v1.0**: Basic SEO monitoring

---

**🎯 You now have a complete, production-ready SEO monitoring system that scales with your data and provides actionable insights for SEO optimization!**