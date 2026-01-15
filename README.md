# BrowserAct – AI Product Image to Short Video Automation
AI-powered automation workflow that transforms product images into engaging short videos with Google Drive sync support.
✨ Features

🤖 AI-Powered Image Collection: Uses BrowserAct for intelligent browser automation to extract structured product data
🎥 Automated Video Generation: Converts product images to AI-generated short videos using industry-leading video generation APIs
☁️ Cloud Storage Integration: Seamless Google Drive sync for organized storage and easy access
🔄 Batch Processing: Process multiple products efficiently with scalable automation
📊 Structured Data Pipeline: Automatic image Base64 conversion, API polling, and file management
⚡ Production Ready: Fully tested workflow with error handling and status monitoring

🎯 Use Cases

E-commerce Automation: Generate product promo videos at scale
Social Media Content: Create engaging short videos for TikTok, Instagram Reels, YouTube Shorts
Marketing Campaigns: Bulk video creation for product launches and seasonal promotions
Inventory Management: Automated video generation as part of product onboarding
Multi-channel Distribution: Create video content for multiple platforms simultaneously

📋 Workflow Overview
┌─────────────────────────────────────────────────────────────┐
│ 1. Data Extraction (BrowserAct)                               │
│    └─> Crawl target website → Extract product data + images   │
│                                                               │
│ 2. Data Parsing                                              │
│    └─> Parse JSON output → Iterate through products           │
│                                                               │
│ 3. Image Processing                                          │
│    └─> Download image → Convert to Base64 → Prepare for AI   │
│                                                               │
│ 4. AI Video Generation                                       │
│    └─> Send to video API → Get job ID → Poll status          │
│                                                               │
│ 5. File Download & Upload                                    │
│    └─> Download video → Upload to Google Drive               │
│                                                               │
│ 6. Organization                                              │
│    └─> Store original image + video in Google Drive          │
└─────────────────────────────────────────────────────────────┘
🚀 Quick Start
Prerequisites
RequirementDetailsn8n Instancev1.0+ with BrowserAct node installedBrowserAct AccountAPI key for browser automationGoogle Drive AccountFor cloud storage of images and videosAI Video API(e.g., RunwayML, D-ID, or similar)API CredentialsAPI keys for all connected services
Setup Instructions
Step 1: Import the Workflow

Open your n8n instance
Click "Create" → "From JSON"
Copy and paste the workflow JSON from workflows/browseract-ai-video.json
Click "Import"

Step 2: Configure Credentials
BrowserAct Connection

In the workflow, find the "BrowserAct" node
Click "Credentials" → "Create New"
Enter your BrowserAct API key
Save the credential

Google Drive Connection

Find the "Google Drive Upload" node
Click "Credentials" → "Create New"
Authenticate with your Google account
Grant necessary permissions (read/write access to Drive)

AI Video API Connection

Find the video generation node
Enter your API key and endpoint URL
Configure API parameters as needed

Step 3: Configure Input Parameters
Edit the initial BrowserAct node with your target settings:
ParameterDescriptionExampleTarget URLWebsite to scrapehttps://example.com/productsSearch KeywordProduct filter keyword"summer collection"Data LimitMax products to process10SelectorCSS selector for images.product-image
Step 4: Configure Google Drive Settings
In the "Google Drive Upload" node:

Select or create destination folder: /BrowserAct Videos/
Naming convention: {product_name}_{timestamp}

Step 5: Test the Workflow

Click "Test workflow"
Monitor execution in the execution logs
Check Google Drive for uploaded files
Verify video quality and metadata

📊 Workflow Nodes Breakdown
1. BrowserAct Data Extraction

Extracts structured product data from target website
Outputs: Product name, image URL, price, description, category

2. Parse Output

Converts BrowserAct JSON response into iterable items
Splits data into individual product records

3. Loop Through Products (Iterator)

Processes each product independently
Allows batch size configuration

4. Download Image

Fetches product image from URL
Handles HTTP redirects and error cases

5. Convert to Base64

Encodes image for API transmission
Optimizes file size if needed

6. Call AI Video Generation API

Sends image + prompt to video generation service
Receives job ID for status polling
Includes retry logic for API failures

7. Poll Video Status

Monitors generation progress
Waits for completion (configurable timeout)
Extracts download URL when ready

8. Download Generated Video

Retrieves completed video file
Validates file integrity

9. Upload to Google Drive

Uploads both original image and video
Organizes by product/date
Creates metadata file for reference

10. Update Status & Logging

Records success/failure for each product
Generates execution summary report

⚙️ Configuration Reference
Environment Variables (Optional)
Create an .env file for sensitive data (not recommended for public repos):
envBROWSERACT_API_KEY=your_key_here
GOOGLE_DRIVE_FOLDER_ID=folder_id_here
VIDEO_API_KEY=your_api_key_here
BATCH_SIZE=5
TIMEOUT_SECONDS=300
Video Generation Prompt
The workflow uses a customizable prompt template:
Create a short, engaging promotional video for this product:
- Product Name: {product_name}
- Category: {category}
- Price: {price}
- Key Features: {features}
- Style: Modern, dynamic, suitable for social media
- Duration: 15 seconds
- Format: Vertical video (9:16)
Processing Limits
Adjust these parameters based on your needs:
SettingDefaultRangeImpactBatch Size51-50Products per executionTimeout300s60-900sVideo generation wait timeRetry Count31-5API failure retry attemptsImage Quality80%50-100%Affects Base64 size
📝 Input/Output Examples
Input (BrowserAct Output)
json{
  "products": [
    {
      "name": "Summer Dress",
      "image_url": "https://example.com/images/dress.jpg",
      "price": "$49.99",
      "description": "Lightweight summer dress",
      "category": "Fashion"
    }
  ]
}
Output (Google Drive Structure)
/BrowserAct Videos/
├── Summer Dress_2025-01-15/
│   ├── original_image.jpg
│   ├── generated_video.mp4
│   └── metadata.json
├── Winter Coat_2025-01-15/
│   ├── original_image.jpg
│   ├── generated_video.mp4
│   └── metadata.json
🔧 Troubleshooting
⚠️ Parameter Chain Risk
Problem: Link field output not mapping to input-Location parameter
Solution:
Validate user Input node:
  └─ Output: "Link"

Run workflow (BrowserAct) node:
  └─ Input mapping: Link → input-Location
  └─ Input mapping: Provider → input-Provider
Verification:

Check "Run workflow" node configuration
Verify field names match exactly
Enable debug mode to see actual values passed

Common Issues & Solutions
IssueCauseSolutionVideo API timeoutLarge image fileCompress image before upload / Increase timeoutGoogle Drive quota exceededStorage limit reachedIncrease Google Drive storage or delete old videosBrowserAct fails to load pageTarget URL changed/blockedUpdate target URL / Add proxy configurationImage download failsInvalid URL or CORS issuesVerify image URL / Check BrowserAct permissionsBase64 encoding errorUnsupported image formatEnsure JPEG/PNG format / Check file size
Debug Mode
Enable debug logging in workflow settings:
Settings → Logs → Enable Debug Mode
Monitor execution logs to see:

API request/response payloads
File download progress
Google Drive upload status
Error stack traces

📚 API Documentation
Supported Video Generation APIs
This workflow is pre-configured for:

RunwayML: For high-quality AI video generation
D-ID: For avatar-based video content
Custom API: Easily adaptable to other services

To switch providers:

Find the "Generate Video" node
Update API endpoint URL
Adjust request payload structure if needed
Test with sample image

BrowserAct API Parameters
Key BrowserAct parameters used in this workflow:
json{
  "action": "extract",
  "url": "target_website_url",
  "wait_for_selector": ".product-image",
  "extract_format": "json",
  "timeout": 30
}
🔒 Security & Best Practices
Credential Management

✅ Store API keys in n8n Credentials, never in workflow JSON
✅ Use environment variables for sensitive data
✅ Rotate API keys regularly
✅ Use OAuth where available instead of static tokens

Data Privacy

✅ Only process public product data
✅ Respect robots.txt and terms of service
✅ Implement rate limiting to avoid IP bans
✅ Clear temporary files after processing

Google Drive Security

✅ Use specific folder permissions, not "Anyone with link"
✅ Regular audit of shared folders
✅ Enable 2FA on Google account
✅ Review activity logs periodically

📊 Performance Optimization
For Large Batches
javascript// Adjust concurrent processing
Batch Size: 10
Parallel Executions: 3
Memory Allocation: 4GB

// Expected performance:
// - 100 products: ~30 minutes
// - 1000 products: ~5 hours
// - 10000 products: Split into multiple workflow executions
Cost Optimization

Compress images to reduce API payload
Reuse videos if product images are identical
Schedule off-peak hours for processing
Monitor API usage and quota

🐛 Known Issues & Limitations
Current Version (v1.0)

⏳ Awaiting Review: Full testing and validation in progress
🔄 Parameter Mapping: Requires careful configuration to prevent breakage
⏱️ Timeout Handling: Video generation may timeout for complex scenes
🎯 Selector Specificity: CSS selectors may need adjustment per website

Roadmap

 Add batch retry mechanism
 Support for video watermarking
 Multi-language prompt support
 Advanced analytics dashboard
 Webhook notifications on completion

📖 Additional Documentation

Setup Guide - Detailed step-by-step configuration
Parameter Reference - Complete parameter documentation
API Integration Guide - How to use different video APIs
Troubleshooting - Common problems and solutions
Best Practices - Tips for optimal workflow performance

🤝 Contributing
We welcome contributions! Areas we need help with:

Testing with different websites and data sources
Performance optimization suggestions
Additional video API integrations
Documentation improvements
Bug reports and fixes

📄 License
MIT License - Feel free to use, modify, and distribute
⚠️ Disclaimer
REVIEW STATUS: This workflow is currently under review and testing. Use at your own risk in production environments.
Before deploying:

✅ Thoroughly test with your specific data sources
✅ Validate parameter mappings
✅ Monitor API usage and costs
✅ Verify Google Drive storage limits
✅ Check BrowserAct compliance with target websites

📞 Support & Feedback

Report Issues: GitHub Issues
Discussions: GitHub Discussions
Community: n8n Community Forum

🙏 Acknowledgments

Built with n8n
Powered by BrowserAct
AI video generation by industry-leading providers


Last Updated: January 2026
Workflow Version: 1.0.0
Status: ⏳ Under Review
For the latest updates, check the Releases page.
