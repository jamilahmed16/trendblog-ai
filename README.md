# TrendBlog AI

An AI editorial tool that turns live sports trends into research-backed, SEO and GEO ready articles. It finds what is trending, pulls real SERP and People Also Ask data, grounds Claude in reference sources you choose, scores the draft for AI-search extractability, and exports publish-ready HTML with schema markup.

Built for [BestSportsMag](https://bestsportsmag.com), part of the FarSightX Media Group, by [Jamil Ahmed](https://jamilahmed.com) at [Reinforce Lab](https://reinforcelab.com).

## What it does

| Stage | What happens |
|---|---|
| Trends | Pulls current topics from Brave News, Google News or Google Trends (via SerpAPI), with a curated fallback list |
| Research | Fetches SERP results and People Also Ask questions for the chosen topic, with optional keyword data from DataForSEO |
| References | Fetches up to 5 source URLs you provide and extracts their text so the model can cite real facts |
| Generate | Claude writes the article using only the supplied sources, in one of several article templates |
| Score | Rates the draft on named entities, source attribution, definitional clarity, specific numbers, extractable claims and direct-answer opening |
| Fix | One click rewrites a single weak area without touching the rest of the article |
| Export | Outputs styled HTML with `Article` and `FAQPage` schema, ready to post to WordPress |

## Why it is built this way

- **Evidence first.** The model is told that every statistic, name, date and quote must come from the supplied sources, and to mention both sides when sources conflict.
- **GEO aware.** The scoring step checks the signals AI search engines lift: clear definitions, attributed claims and quotable facts.
- **Server holds the secrets.** The browser app never sees an API key. All paid calls go through the server.

## Architecture

```text
Browser app (app/index.html)
        |  x-app-token
        v
Express server (server/server.js)
        |-- Anthropic API   (article generation, section fixes)
        |-- Brave / SerpAPI (trends, SERP, People Also Ask)
        |-- DataForSEO      (keyword data, optional)
        '-- Reference URLs  (public pages only)
```

## Quick start

```bash
cd server
cp .env.example .env        # fill in APP_TOKEN and ANTHROPIC_API_KEY
npm install
export $(grep -v '^#' .env | xargs)
npm start                   # http://localhost:3000
```

In a second terminal, serve the app from an allowed origin:

```bash
cd app
python3 -m http.server 8080
```

Open `http://localhost:8080`, set the proxy URL to `http://localhost:3000`, and paste your `APP_TOKEN` into Quick Settings.

## Configuration

| Variable | Required | Purpose |
|---|---|---|
| `APP_TOKEN` | Yes | Shared secret the app sends in the `x-app-token` header |
| `ANTHROPIC_API_KEY` | Yes | Article generation and section fixes |
| `ALLOWED_ORIGINS` | Yes in production | Comma separated list of origins allowed by CORS |
| `BRAVE_API_KEY` | Recommended | Live news trends |
| `SERP_API_KEY` | Recommended | SERP data and People Also Ask |
| `DATAFORSEO_LOGIN`, `DATAFORSEO_PASSWORD` | Optional | Keyword volume and difficulty |
| `RATE_LIMIT_PER_MIN` | Optional | Requests per IP per minute, default 30 |

## Security

The server fails closed: without `APP_TOKEN` it refuses requests. CORS is an allowlist, requests are rate limited, output caps protect your API bill, and the reference fetcher refuses private and local addresses. See [SECURITY.md](SECURITY.md).

## Adapting it to your niche

The article templates, brand name, byline and schema publisher are set for BestSportsMag. Edit the template functions in `server/server.js` and the Quick Settings defaults in `app/index.html` to point it at your own publication.

## Want this built for your brand?

I build AI content and growth systems like this for B2B brands and publishers. Start at [reinforcelab.com](https://reinforcelab.com) or [jamilahmed.com](https://jamilahmed.com).

## License

MIT. See [LICENSE](LICENSE).
