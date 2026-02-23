📊 Slack to Google Drive HTML Report Generator
An automated workflow that converts Slack messages into beautifully styled HTML reports using SerpAPI and OpenAI, then delivers them to Google Drive with Slack notifications.

🚀 Features
Slack Integration: Trigger reports via simple Slack messages

Web Search: Automatic web research using SerpAPI

AI-Powered Reports: Generate comprehensive HTML reports with OpenAI

Beautiful Styling: Modern, responsive HTML with Tailwind CSS and custom gradients

Interactive Elements: Hover effects, animations, and color-coded tables

Google Drive Storage: Automatic upload to designated Drive folder

Smart Filtering: Prevents infinite loops by ignoring bot messages

Slack Notifications: Get shareable links directly in your channel

# 📧 Gmail Integration for HTML Report Automation

This document covers the Gmail integration part of your Slack-to-Google-Drive HTML report automation workflow.

📋 Prerequisites
n8n instance (self-hosted or cloud)

Slack workspace with admin access

SerpAPI API key

OpenAI API key

Google Drive account

gmail account

🔧 Installation
1. Create a Slack App
Go to api.slack.com/apps

Click "Create New App"

Choose "From scratch"

Add these Bot Token Scopes:

chat:write

files:write

channels:history

channels:read

Install app to workspace

Copy Bot User OAuth Token

2. Set Up Google Drive
Create a folder named "slack-reports" in Google Drive

Get folder ID from URL: https://drive.google.com/drive/folders/YOUR_FOLDER_ID

Share folder with n8n service account (Editor access)

3. Configure n8n Credentials
Create these credentials in n8n:

Slack: Bot User OAuth Token

SerpAPI: API key

OpenAI: API key

Google Drive: OAuth2 or Service Account

📁 Workflow Structure
text
Slack Trigger 
    ↓
IF Filter (Prevent Bot Loops)
    ↓
SerpAPI Search (Web Research)
    ↓
OpenAI (Generate HTML Content)
    ↓
HTML Formatter (Add Styling & Tables)
    ↓
Convert to File (Create .html File)
    ↓
Google Drive Upload
    ↓
Link Generator (Create Shareable Links)
    ↓
Slack Notification (Post Links)
    ↓
gmail
    
💻 Code Components
1. Bot Filter (After Slack Trigger)
javascript
// Prevents infinite loops by ignoring bot messages
const slackData = $json['Stack Trigger'] || $json;
const BOT_ID = 'YOUR_BOT_ID';
const BOT_USER_ID = 'YOUR_BOT_USER_ID';

const isBot = slackData.bot_id === BOT_ID || 
              slackData.user === BOT_USER_ID ||
              slackData.subtype === 'bot_message';

if (isBot) return [];
return [{ json: slackData }];
2. HTML Formatter (After OpenAI)
javascript
// Extracts and beautifies HTML from OpenAI response
let htmlContent = inputData.output?.[0]?.content?.[0]?.text;
// Clean and style the HTML...
3. Google Drive Link Generator
javascript
// Creates shareable Drive links
const fileId = driveOutput.id;
const viewLink = `https://drive.google.com/file/d/${fileId}/view`;
const downloadLink = `https://drive.google.com/uc?export=download&id=${fileId}`;
🎨 Styling Features
Gradient Backgrounds: Modern purple gradient themes

Interactive Tables: Hover effects, column-specific colors

Responsive Design: Mobile-optimized layouts

Card Layouts: Beautiful content cards with shadows

Animations: Smooth transitions and loading effects

Icons: Font Awesome integration

📝 Usage
In Slack, go to your configured channel

Send a message like: generate report on artificial intelligence

Wait 10-20 seconds for processing

Receive a link to your styled HTML report in Google Drive

Click to view the beautifully formatted report

⚙️ Configuration Options
Slack Trigger Settings
Channel: Select your monitoring channel

Trigger On: New Message Posted to Channel

Watch Whole Workspace: Optional

SerpAPI Settings
Query: {{ $json.text }} (from Slack message)

Location: Your preferred search location

Results: Number of results to return

OpenAI Settings
Model: gpt-4 or gpt-3.5-turbo

Prompt: Customize report generation instructions

Temperature: 0.7 for balanced creativity

🐛 Troubleshooting
Common Issues
Issue	Solution
Infinite Loop	Add bot filter code after Slack Trigger
HTML Shows as Text	Ensure Convert to File uses 'data' field
Google Drive Upload Fails	Check folder sharing permissions
Missing Icons	Font Awesome CDN might be blocked
Slow Generation	Reduce SerpAPI result count
Debug Mode
Add temporary console logs:

javascript
console.log('Data structure:', Object.keys($json));
console.log('Bot ID:', $json.bot_id);
🔒 Security Notes
Store all API keys in n8m credentials, never in code

Use Slack signing secret for request verification

Restrict Google Drive folder access to n8n only

Regularly rotate API keys

📊 Performance
Average Response Time: 10-20 seconds

File Size: 5-50 KB per report

Concurrent Users: Depends on n8n instance capacity

API Limits: Respect rate limits for all services

🚀 Advanced Customizations
Add Dark Mode
css
@media (prefers-color-scheme: dark) {
  body { background: #1a1a1a; }
  .container { background: #2d2d2d; }
}
Include Charts
Add Chart.js integration:

html
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<canvas id="myChart"></canvas>
Email Reports
Add an email node after Google Drive to send reports via email

📈 Monitoring
Use n8m execution logs to track performance

Set up alerts for failed executions

Monitor API usage in respective dashboards

Track Google Drive storage usage

🤝 Contributing
Fork the repository

Create feature branch

Commit changes

Push to branch

Open pull request

📄 License
MIT License - use freely for personal and commercial projects

🙏 Acknowledgments
n8n for the automation platform

OpenAI for GPT models

SerpAPI for search capabilities

Google Drive for cloud storage

Slack for team communication

📞 Support
Issues: GitHub Issues

Documentation: n8n.io/docs

Community: community.n8n.io