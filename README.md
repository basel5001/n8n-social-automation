# n8n Social Media Automation

Automated social media publishing across multiple platforms using [n8n](https://n8n.io/) and AI-generated content.

## Features

- **AI Content Generation** - Uses OpenAI GPT-4o-mini to generate engaging posts
- **Multi-Platform Publishing** - Posts to Facebook, Twitter/X, LinkedIn, and Instagram
- **Scheduled Posting** - Configurable cron schedules (default: daily at 9 AM)
- **Batch Content** - Generate multiple posts at once with varied tones
- **Self-Hosted** - Runs via Docker Compose, your data stays with you

## Architecture

```
Schedule Trigger (Cron)
    |
    v
OpenAI GPT-4o-mini (Content Generation)
    |
    v
Format for Platforms (Trim for Twitter, etc.)
    |
    +---> Facebook Graph API
    +---> Twitter/X API v2
    +---> LinkedIn UGC API
    +---> Instagram Graph API
```

## Quick Start

### Prerequisites

- Docker & Docker Compose
- API credentials for platforms you want to use
- OpenAI API key

### Setup

```bash
# Clone
git clone https://github.com/basel5001/n8n-social-automation.git
cd n8n-social-automation

# Configure
cp .env.example .env
# Edit .env with your API keys and credentials

# Start
docker compose up -d

# Open n8n UI
open http://localhost:5678
```

### Import Workflows

1. Open n8n at `http://localhost:5678`
2. Go to **Workflows** > **Import from File**
3. Import `workflows/social-media-publisher.json`
4. (Optional) Import `workflows/content-queue-publisher.json`
5. Configure credentials in n8n for each platform
6. Activate the workflow

## Workflows

| Workflow | Schedule | Description |
|----------|----------|-------------|
| `social-media-publisher.json` | Daily 9 AM | Single AI post to all platforms |
| `content-queue-publisher.json` | 12 PM & 6 PM | Batch of 3 varied posts |

## Platform Setup

### Facebook Pages

1. Create a [Facebook App](https://developers.facebook.com/)
2. Add **Pages** product
3. Generate a long-lived Page Access Token
4. Set `FACEBOOK_PAGE_ID` and `FACEBOOK_ACCESS_TOKEN` in `.env`

### Twitter/X

1. Create a [Twitter Developer](https://developer.twitter.com/) app
2. Enable OAuth 2.0
3. Generate access tokens with tweet.write scope
4. Configure OAuth2 credentials in n8n

### LinkedIn

1. Create a [LinkedIn App](https://www.linkedin.com/developers/)
2. Request `w_member_social` permission
3. Generate access token via OAuth flow
4. Set `LINKEDIN_ACCESS_TOKEN` and `LINKEDIN_PERSON_URN` in `.env`

### Instagram

1. Connect Instagram Business account to a Facebook Page
2. Use the same Facebook App from above
3. Get Instagram Business Account ID via Graph API
4. Set `INSTAGRAM_BUSINESS_ID` and `INSTAGRAM_ACCESS_TOKEN` in `.env`

## Configuration

| Variable | Description | Required |
|----------|-------------|----------|
| `N8N_ENCRYPTION_KEY` | Encrypts stored credentials | Yes |
| `OPENAI_API_KEY` | OpenAI API key for content generation | Yes |
| `FACEBOOK_PAGE_ID` | Facebook Page ID | For Facebook |
| `FACEBOOK_ACCESS_TOKEN` | Long-lived Page access token | For Facebook |
| `TWITTER_*` | Twitter OAuth credentials | For Twitter |
| `LINKEDIN_ACCESS_TOKEN` | LinkedIn OAuth token | For LinkedIn |
| `INSTAGRAM_BUSINESS_ID` | Instagram Business Account ID | For Instagram |

## Customization

### Change AI Prompts

Edit the OpenAI node prompt in the workflow JSON or via the n8n UI to customize:
- Topics (default: tech, DevOps, cloud, cybersecurity)
- Tone (educational, conversational, promotional)
- Post length
- Hashtag strategy

### Change Schedule

Edit the cron expression in the Schedule Trigger node:
- `0 9 * * *` - Daily at 9 AM
- `0 9,18 * * 1-5` - Weekdays at 9 AM and 6 PM
- `0 */4 * * *` - Every 4 hours

## Production Deployment

For production, consider:

1. **Use HTTPS** - Set `N8N_PROTOCOL=https` and configure a reverse proxy (nginx/Caddy)
2. **Backup credentials** - The `N8N_ENCRYPTION_KEY` is critical for credential recovery
3. **Monitor** - Set up health checks and alerting on failed workflow executions
4. **Rate limits** - Respect platform API rate limits (especially Twitter: 50 tweets/day)

## License

MIT
