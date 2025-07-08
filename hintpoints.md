# SEO Watchdog Project - VPS UK

## Repository Information
- **GitHub URL**: https://github.com/rolanbendecio/seowatchdog
- **Project Type**: Automated SEO monitoring workflow built with n8n
- **Location**: VPS UK

## Project Overview
SEO Watchdog is an automated SEO monitoring workflow that:
- Pulls Google Search Console data from BigQuery
- Analyzes performance metrics using Claude AI
- Generates daily SEO insights reports
- Sends formatted reports to Telegram

## Core Components
1. Scheduler (runs daily at 9 AM)
2. BigQuery data extraction
3. Claude AI analysis
4. Report formatting
5. Telegram notification delivery

## Report Sections Include
- Overall site performance summary
- Traffic trends
- Low CTR page opportunities
- Indexing issues
- Actionable SEO recommendations

## Prerequisites
- Google Cloud Platform account
- BigQuery API access
- Anthropic Claude AI API key
- Telegram bot
- n8n workflow platform

## Customization Options
- Adjust report schedule
- Modify BigQuery queries
- Customize AI analysis prompts
- Configure alert thresholds

## To Continue Working
To work with this repository locally, run:
```bash
git clone https://github.com/rolanbendecio/seowatchdog
cd seowatchdog
```

## Development Notes
- This project is set up on VPS UK
- All project files and configurations should be stored in /home/devuser/seowatchdog/