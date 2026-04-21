# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Run the scraper
go run main.go

# Build a binary
go build -o scraper .

# Download dependencies
go mod download

# Tidy dependencies
go mod tidy
```

There are no tests in this project.

## Architecture

This is a single-file Go web scraper (`main.go`) using the [Colly](https://github.com/gocolly/colly) framework to scrape quotes from `quotes.toscrape.com`.

The scraper is structured around Colly's event-driven callback model:
- `OnRequest` — sets a Mozilla user-agent header before each request
- `OnResponse` — logs HTTP status
- `OnError` — logs errors (non-fatal; scraping continues)
- `OnHTML` — extracts `.quote` elements and prints quote text + author

The collector is domain-restricted to `quotes.toscrape.com` to prevent accidental off-domain requests.

Output format per quote:
```
"[quote text]"
By [author]
```

## Dependencies

- **github.com/gocolly/colly v1.2.0** — the only direct dependency; handles HTTP, HTML parsing, and robots.txt compliance internally via indirect deps (goquery, antchfx/htmlquery, temoto/robotstxt, etc.)
- Go 1.21.4
