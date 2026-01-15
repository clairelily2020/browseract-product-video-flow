# 🎬 BrowserAct – AI Product Image to Short Video Automation

Convert product images into engaging AI-generated short videos with automated workflow. Extract product data and images from websites, then automatically generate and upload videos to Google Drive.

## ✨ What It Does

This workflow automates the entire process of turning product images into short videos:

1. **Extract product data** from websites using BrowserAct (gets URLs and product info)
2. **Download product images** from the extracted URLs
3. **Convert to Base64** for API processing
4. **Generate AI videos** from product images using video generation API
5. **Monitor generation status** until videos are ready
6. **Upload everything** (original images + videos) to Google Drive

All in one automated workflow.

## 🎯 Use Cases

- Bulk create product promo videos for e-commerce sites
- Generate social media content (TikTok, Instagram Reels, YouTube Shorts)
- Automated video creation for product launches
- Mass video generation for inventory
- Content distribution across multiple platforms

## 🚀 Quick Start

### What You Need

- n8n workflow editor
- BrowserAct account (to extract product URLs)
- Google Drive account (to store images and videos)
- Video generation API account (RunwayML, D-ID, or similar)

### Setup (4 Steps)

**1. Import the workflow**
- Copy the workflow JSON into your n8n instance
- Click "Import from JSON"

**2. Add credentials**
- **BrowserAct**: Add your API key
- **Google Drive**: Authorize your account
- **Video API**: Add your API key and endpoint

**3. Configure the nodes**
- **BrowserAct node**: Set the website URL to scrape
- **limit_products**: Set how many products to process
- **set_prompt**: Customize the video generation prompt
- **Google Drive upload**: Choose your destination folder

**4. Run**
- Click "Test workflow" or "Execute"
- Check Google Drive for uploaded files

## 📋 How It Works

### The Workflow Steps

1. **Execute trigger** - Start the workflow manually or on schedule
2. **BrowserAct extraction** - Gets product URLs and data from website
3. **Code node** - Limits the number of products to process
4. **Iterator** - Loops through each product one by one
5. **Fetch image** - Downloads the product image from URL
6. **Convert to Base64** - Encodes image for API
7. **Set prompt** - Creates AI prompt for video generation
8. **HTTP Request** - Sends image to video API, gets job ID
9. **Wait** - Pauses to let video generation start
10. **Fetch status** - Checks if video is ready
11. **Check response** - If ready, proceeds; if not, waits and retries
12. **Download video** - Downloads completed video file
13. **Upload files** - Uploads both image and video to Google Drive

Simple loop: fetch → convert → generate → upload

## ⚙️ Key Configuration

### BrowserAct Node

BrowserAct extracts product data from your target website:
- Provides: Product names, descriptions, URLs
- Note: You need to provide image URLs separately (BrowserAct extracts URLs, not images directly)

### Video API

Send images and prompts to generate videos:
- Supports: RunwayML, D-ID, or any video generation API
- Input: Image (Base64) + Text prompt
- Output: Video file URL

### Google Drive Upload

Creates organized folder structure:
- Format: `ProductName_Date/`
- Contains: Original image + Generated video + Metadata

## 🔧 Customization

**Change the website to scrape**: Edit the BrowserAct node URL

**Adjust batch size**: Change `limit_products` value (recommend 5-10 for testing)

**Customize video prompt**: Edit `set_prompt` node with your desired video description template

**Change video duration**: Modify API parameters in the HTTP Request node

**Organize files differently**: Adjust the Google Drive upload path structure

## 🐛 Common Issues

**"BrowserAct cannot find product URLs"**
- Verify the website URL is correct
- Check if the website structure has changed
- Some websites may block automated access

**"Image download fails"**
- Ensure product image URLs are valid
- Check if images are publicly accessible
- Verify no CORS restrictions

**"Video generation times out"**
- Increase the Wait node timeout value
- API may be slow - try increasing retry attempts
- Reduce batch size to process fewer products

**"Google Drive upload fails"**
- Check storage quota
- Verify write permissions
- Ensure folder path exists

## 💡 Tips

- **Test first**: Start with 2-3 products to verify workflow
- **Monitor logs**: Check execution logs to spot issues
- **Schedule it**: Set up triggers to run daily/weekly
- **Batch size**: 5-10 products is optimal for stability
- **Video quality**: Depends on your API provider's plan

## 🔒 Security

- Store API keys in n8n Credentials, never in workflow
- Use environment variables for sensitive data
- Remove credentials before sharing workflow JSON
- Keep API keys rotated and updated

## ⚠️ Important Notes

- This workflow is under review and testing
- Test thoroughly before production use
- Video generation API costs apply (typically $0.30-0.50 per video)
- Google Drive storage limits apply
- Processing time depends on API and batch size

## 📊 Expected Performance

- Per product: 2-5 minutes
- Batch of 10: 30-60 minutes
- Depends on: API response time, image size, video complexity

## 📄 License

MIT License

## 📞 Need Help?

Check the workflow execution logs for detailed error messages. Most issues come from:
1. Missing or incorrect credentials
2. Website structure changes
3. API rate limiting
4. Storage quota exceeded

---

**Version**: 1.0.0  
**Created**: January 2026  
**Status**: ⏳ Under Review
