# book-research

AI-assisted search and download of books from shadow libraries (Anna's Archive, Libgen+) using GitHub Copilot and playwright-cli.

## Setup

1. Install [playwright-cli](https://github.com/microsoft/playwright-cli):
   ```bash
   npm install -g @playwright/mcp
   ```

2. Install the playwright-cli skill so Copilot knows how to use it:
   ```bash
   playwright-cli install --skills https://github.com/microsoft/playwright-cli
   ```
