# Snap Issue privacy policy

Last updated: 13 November 2026

Snap Issue is a Chrome extension that turns screenshots of web pages into GitHub issues. It has no server of its own. The developer does not receive, store or sell any of your data, and the extension contains no analytics or tracking.

## What the extension handles

- **Screenshots** of pages you choose to capture, with any annotations you add.
- **Page details** recorded with each screenshot: the page URL and title, browser and OS version, window size and pixel ratio.
- **The issue you write**: title, body, labels, assignees and milestone.
- **Credentials you provide**: your GitHub sign-in tokens, AI provider API keys and image host keys.
- **Settings**: your chosen AI provider and model, output language, image host address and default repository per site.

The extension captures a page only when you click the toolbar button, use a keyboard shortcut or choose the right-click menu item. It does not read or record pages in the background.

## Where it goes

Data leaves your computer only to services you set up, and only when you act:

- **GitHub** (`api.github.com`, `github.com`): when you sign in, pick a repository, search for duplicates or create an issue or comment. The issue holds what you wrote and links to your screenshots.
- **Your AI provider**: when you click Generate, the screenshots, page details and your description go to the provider you chose:
  - Anthropic (`api.anthropic.com`) or OpenAI (`api.openai.com`) with your own API key.
  - The Claude Code or Codex app on your computer, through the optional local helper, which uses your own sign-in.

  Each provider's own privacy policy covers what it does with the request.
- **Your image host**: when you create an issue, screenshots are uploaded to the S3-compatible bucket you set up in Options. Anyone with the image link can view it, like any image in a GitHub issue.

Blurred areas are baked into the image on your computer before anything is uploaded or sent to an AI provider.

## What is stored, and where

- **On this computer only** (`chrome.storage.local` and IndexedDB): the open draft and its screenshots, GitHub tokens, API keys, image host keys and the list of issues you created.
- **In Chrome sync** (`chrome.storage.sync`, if you have Chrome sync on): non-secret settings, such as AI provider and model, language, image host address and default repositories.

Creating an issue clears its draft. Removing the extension deletes everything it stored locally. Options has a button to clear the issue history.

## Permissions

The extension asks for access to other sites only when you need it: an AI provider's API when you save its key, and your image host's address when you save it.

## Changes and contact

Changes to this policy are published at this address with a new date. For questions, open an issue in the Snap Issue repository or contact the developer through the Chrome Web Store listing.
