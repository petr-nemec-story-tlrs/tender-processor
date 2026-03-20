# Research Agents — Detailed Prompts & Configuration

This document contains the exact prompts for dispatching the 5 parallel research agents in Step 2. Each prompt is designed to be self-contained so agents can work independently.

## Agent 1: Profile Scanner

**Goal:** Map the client's social media presence across all platforms.

**Prompt template:**
```
Research the social media presence of {CLIENT_NAME} ({BRAND_NAME}) across ALL platforms.

For each platform found, collect:
- Profile URL
- Follower/subscriber count
- Number of posts/videos
- Account creation date (if visible)
- Bio/description text
- Posting frequency (posts per week/month)
- Content language(s)

Platforms to check:
1. Instagram (search: @{handle}, {brand} kombucha, {brand} czech)
2. TikTok
3. Facebook (page + any groups)
4. LinkedIn (company page)
5. YouTube
6. X/Twitter
7. Pinterest

Use `apify-slash-rag-web-browser` to search for "{CLIENT_NAME} {BRAND_NAME} social media" and "{BRAND_NAME} instagram/tiktok/facebook".

For Instagram specifically, try using `call-actor` with actor `apify/instagram-profile-scraper` with input:
{
  "usernames": ["{instagram_handle}"]
}

Save findings as structured text. End with:
- **Platforms found:** list
- **Platforms not found:** list
- **Primary platform:** which one has most activity
- **Key observation:** one-sentence summary
```

## Agent 2: Deep Platform Analyst

**Goal:** Deep-dive into the client's primary social platform (usually Instagram).

**Prompt template:**
```
Perform a deep analysis of {CLIENT_NAME}'s Instagram account @{instagram_handle}.

Use `call-actor` with actor `apify/instagram-post-scraper` with input:
{
  "username": ["{instagram_handle}"],
  "resultsLimit": 100
}

If the scraper doesn't work, use `apify-slash-rag-web-browser` to analyze the profile page and recent posts manually.

Analyze:
1. **Content mix:** Percentage breakdown of static images, carousels, Reels/video, Stories highlights
2. **Themes:** What topics/subjects dominate (product shots, lifestyle, recipes, UGC, influencer reposts, behind-the-scenes, educational)
3. **Visual style:** Color palette, photography style, graphic design elements, consistency
4. **Copy style:** Tone of voice, hashtag strategy, CTA patterns, emoji usage, language
5. **Engagement analysis:**
   - Average likes per post type
   - Average comments per post type
   - Best-performing posts (top 5 by engagement)
   - Worst-performing posts (bottom 5)
   - Engagement rate calculation: (avg likes + avg comments) / followers * 100
6. **Comment sentiment:** Read through 50+ comments. What do people say? Positive/negative/questions?
7. **Posting cadence:** Posts per week, best days/times, any gaps in activity
8. **Influencer content:** Any tagged/reposted influencer content? How does it perform vs. brand content?
9. **Paid indicators:** Any posts that look sponsored/boosted? "Sponsored" labels?

End with:
- **Content quality score:** 1-10 with justification
- **3 biggest content gaps**
- **3 quick wins** (things that could improve immediately)
- **Benchmark comparison** (vs. typical account this size in this category)
```

## Agent 3: Competitor Mapper

**Goal:** Identify and analyze 5-10 competitors on social media.

**Prompt template:**
```
Identify and analyze the social media presence of {CLIENT_NAME}'s competitors in the {INDUSTRY} space in {MARKET} (Czech Republic).

Step 1: Identify competitors
Use `apify-slash-rag-web-browser` to search for:
- "{INDUSTRY} Czech Republic brands"
- "{INDUSTRY} Česko značky"
- "best {PRODUCT} brands Czech"
- "{PRODUCT} competition {MARKET}"

Compile a list of 8-12 competitors.

Step 2: For each competitor, collect:
- Brand name
- Instagram handle + follower count
- TikTok handle + follower count (if exists)
- Facebook page + follower count
- Content strategy summary (2-3 sentences)
- Posting frequency
- Engagement level (low/medium/high)
- Unique positioning or differentiator
- Notable campaigns or content series

Step 3: Create comparative table:
| Brand | IG Followers | TT Followers | FB Followers | Strategy | Engagement | Differentiator |

Step 4: Deep-dive top 3 competitors:
For the 3 strongest competitors, analyze their Instagram in detail:
- Content themes and formats
- Visual identity
- Influencer partnerships (visible)
- Paid ad activity (if visible)
- Community engagement approach

End with:
- **Market leader:** who dominates and why
- **Whitespace:** what no one is doing well
- **Threats:** which competitors could be problematic
- **Opportunities:** positioning gaps {CLIENT_NAME} can exploit
```

## Agent 4: Media & Community Scanner

**Goal:** Find all mentions of the client across web, news, and community platforms.

**Prompt template:**
```
Search for all mentions, articles, and discussions about {CLIENT_NAME} / {BRAND_NAME} across the internet.

Use `apify-slash-rag-web-browser` for each search query:

Web searches:
- "{BRAND_NAME}" (exact match)
- "{BRAND_NAME} {PRODUCT}"
- "{BRAND_NAME} Czech" / "{BRAND_NAME} Česko"
- "{CLIENT_COMPANY_NAME}" (parent company if different)
- "{BRAND_NAME} review" / "{BRAND_NAME} recenze"

News/PR searches:
- "{BRAND_NAME}" site:forbes.cz OR site:e15.cz OR site:lidovky.cz OR site:aktualne.cz
- "{BRAND_NAME}" site:mediaguru.cz OR site:markomu.cz OR site:tyinternety.cz

Reddit searches:
Use `call-actor` with actor `trudax/reddit-scraper-lite` or search Reddit via web:
- "{BRAND_NAME}" site:reddit.com
- "{PRODUCT} Czech" site:reddit.com
- "{PRODUCT} best brand" site:reddit.com

Forum/Community searches:
- "{BRAND_NAME}" site:facebook.com/groups
- "{PRODUCT}" Czech forums, discussion boards

For each mention found, record:
- Source (URL)
- Date
- Context/snippet
- Sentiment (positive/neutral/negative)
- Reach estimate (site traffic level)

End with:
- **Total mentions found:** count
- **Sentiment breakdown:** positive/neutral/negative percentages
- **Media visibility score:** 1-10 (1 = invisible, 10 = well-known)
- **Key narrative:** what story does the internet tell about this brand?
- **PR opportunity:** what angles could generate media coverage?
```

## Agent 5: Paid Ads Researcher

**Goal:** Analyze paid advertising activity for the client and top competitors.

**Prompt template:**
```
Research the paid advertising activity of {CLIENT_NAME} and their top competitors in the {INDUSTRY} space.

Step 1: Meta Ad Library
Use `apify-slash-rag-web-browser` to check:
- https://www.facebook.com/ads/library/?q={BRAND_NAME}
- https://www.facebook.com/ads/library/?q={COMPETITOR_1}
- https://www.facebook.com/ads/library/?q={COMPETITOR_2}
- https://www.facebook.com/ads/library/?q={COMPETITOR_3}

For each brand with active ads, note:
- Number of active ads
- Ad formats (image, video, carousel)
- Ad copy themes/messages
- Landing page URLs
- Estimated start dates
- Geographic targeting (if visible)

Step 2: Google Ads (if detectable)
Search for "{BRAND_NAME}" and competitors on Google to check for:
- Search ads (branded and generic terms)
- Shopping ads
- Display network presence

Step 3: Influencer/Sponsored Content
Search for sponsored/paid posts:
- "#{BRAND_NAME}ad" or "#{BRAND_NAME}spoluprace" on Instagram
- Influencer disclosure tags

End with:
- **Ad spending intensity:** none/low/medium/high per brand
- **Most common ad format:** across the category
- **Key messaging themes:** what do ads focus on?
- **Competitive ad gap:** what no one is advertising but should
- **Recommended paid strategy:** initial approach for {CLIENT_NAME}
```

## Apify Configuration Notes

### Common Actors

| Actor | Use Case | Key Parameters |
|-------|----------|----------------|
| `apify/instagram-profile-scraper` | Profile data | `usernames: ["handle"]` |
| `apify/instagram-post-scraper` | Post data | `username: ["handle"], resultsLimit: 100` |
| `apify/instagram-comment-scraper` | Comments | `directUrls: ["post_url"], resultsLimit: 200` |
| `trudax/reddit-scraper-lite` | Reddit threads | `searchQuery: "term", sort: "relevance"` |
| `apify/facebook-ads-scraper` | Ad Library | `searchQuery: "brand", country: "CZ"` |

### Fallback Strategy

If a specific Apify actor fails or is unavailable:
1. Try `apify-slash-rag-web-browser` with the direct URL
2. Try `search-actors` to find an alternative actor
3. Use web search as last resort
4. Document the gap in research findings

### Rate Limiting

Space Apify calls at least 2-3 seconds apart within a single agent. Across parallel agents this is handled automatically since they're independent processes.
