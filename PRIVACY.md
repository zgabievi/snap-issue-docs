# Snap Issue privacy policy

Last updated: 13 November 2026

Snap Issue is a Chrome extension that turns screenshots and recordings of web pages into GitHub issues. Its only server is a sign-in relay for issue trackers (described below), which passes sign-in codes and tokens through and keeps nothing. The developer does not receive, store or sell any of your data, and the extension contains no analytics or tracking.

## What the extension handles

- **Screenshots** of pages you choose to capture, with any annotations you add.
- **Tab recordings** you choose to make: a video of the tab (no sound), at most 60 seconds long, with an animated GIF of its first seconds and a few still frames.
- **Page details** recorded with each screenshot or recording: the page URL and title, browser and OS version, window size and pixel ratio. For an element capture, also the element's CSS selector, tag name, position and size, and its visible text (or its label when it has none, never the value typed into a form field).
- **Page errors**, only if you turn on error recording: console errors and failed network requests (method, address without its query string, status) on the tab you captured, recorded after the capture until you stop it, file the draft or close the tab. Before anything is stored, query strings, fragments, `Authorization` and cookie values, bearer tokens and token-like values such as `password=` or `api_key=` are replaced, and each entry is cut to 500 characters. Request and response headers and bodies are never read.
- **The issue you write**: title, body, labels, assignees and milestone.
- **Credentials you provide**: your GitHub sign-in tokens, the sign-in tokens or personal API tokens of any issue tracker you connect (Jira, Linear, Asana, Trello; for Jira also your site address and account email), AI provider API keys and image host keys.
- **Settings**: your chosen AI provider and model, output language, image host type and address, Imgur Client-ID, the screenshot branch name and default repository per site.

The extension captures a page only when you click the toolbar button, use a keyboard shortcut or choose the right-click menu item. It records a tab only after you click "Record tab" in the popup, shows REC on the toolbar button while it does, and stops when you click Stop or after 60 seconds. It does not read or record pages in the background. The one exception is error recording, which is off until you turn it on in the editor or in Options. Then, after a capture, it listens to that one tab through Chrome's debugger, and Chrome shows a "started debugging this browser" bar for as long as it does. Click Stop in the editor, or Cancel on that bar, to end it.

## Where it goes

Data leaves your computer only to services you set up, and only when you act:

- **GitHub** (`api.github.com`, `github.com`): when you sign in, pick a repository, search for duplicates or create an issue or comment. The issue holds what you wrote, links to your screenshots and, in a collapsed block, the page errors you left ticked.
- **Jira Cloud** (`api.atlassian.com`, sign-in at `auth.atlassian.com`; with an API token, your site `<name>.atlassian.net` instead): when you connect Jira Cloud, pick a project, search for duplicates or create an issue or comment. The extension reads the Jira sites you allowed and their projects, issue types, priorities, labels, assignable people and active sprints. The issue holds what you wrote and the page errors you left ticked. Screenshots, recordings and their GIF previews are uploaded to Jira as attachments of the issue, so only people who can see the issue can open them, and no image host is used.
- **Linear** (`api.linear.app`), if you connect it: when you pick a team, search for duplicates or create an issue or comment. Screenshots, recordings (the WebM video) and their GIF previews are uploaded to Linear's file storage (Google Cloud Storage, `storage.googleapis.com`) at the one-time upload address Linear gives for each file, and the issue or comment shows them. Linear's access rules decide who can see them, and Linear's privacy policy applies.
- **Your AI provider**: when you click Generate, the screenshots, a few still frames of each recording (never the video), page details, the page errors you left ticked and your description go to the provider you chose:
  - Anthropic (`api.anthropic.com`) or OpenAI (`api.openai.com`) with your own API key.
  - The Claude Code or Codex app on your computer, through the optional local helper, which uses your own sign-in.

  Each provider's own privacy policy covers what it does with the request.
- **Your image host**: when you create a GitHub issue, screenshots, recordings (the WebM video) and their GIF previews are uploaded to the host you picked in Options. The issue shows the GIF and links to the video:
  - An S3-compatible bucket you set up. Anyone with the image link can view it, like any image in a GitHub issue.
  - Imgur (`api.imgur.com`), anonymously, with the Client-ID of an Imgur app you registered. The images are public on Imgur, Imgur's own privacy policy and terms apply, and Imgur may remove them. Don't pick Imgur for private repositories.
  - A branch of the GitHub repository you file the issue to (`api.github.com`), committed with your GitHub sign-in. Only people who can read that repository can see them, and nothing goes to a third party.

- **Asana** (`app.asana.com`), if you connect it: when you pick a project, search for duplicates or create a task or comment. The extension reads your workspaces, projects, tags, sections, project members and custom fields, and searches task names with your title's keywords. The task holds what you wrote, the page errors you left ticked, and your screenshots and recordings as Asana attachments, which follow the project's own access rules. No image host is involved.
- **Trello** (`api.trello.com`, `trello.com`), if you connect it: when you click Connect, Trello's sign-in page returns a token to the extension directly, without the sign-in relay. Then, when you pick a list, search for duplicates or create a card or comment, the extension calls Trello's API: it reads your boards, lists, the board's labels and members, and sends the card's title, description, labels, members and the screenshot, recording and GIF files, which Trello stores as attachments on the card. Trello's own privacy policy covers what it stores.
- **Personal API tokens**: when you connect a tracker with a token you created there (a Jira API token with your email, a Linear personal API key, an Asana personal access token, or your own Trello API key and token), the extension checks it with one call to that tracker and then sends it, in the request's `Authorization` header, only to that tracker's API. It never goes to the sign-in relay or anywhere else.
- **The sign-in relay**: when you connect Jira, Linear or Asana through their sign-in page (not with a token), and when that sign-in is renewed (about once an hour for Jira and Asana, once a day for Linear, while you use it), the extension sends the one-time sign-in code or the refresh token to the developer's relay, a Cloudflare Worker. The relay adds the app's client secret, forwards the request to the tracker's own sign-in server and returns the answer to the extension. It sees the code, the refresh token and the new tokens only in transit. It does not store them, log them or send them anywhere else. Cloudflare handles the connection and its privacy policy applies to it. Screenshots, issues and everything else go from the extension to the tracker directly, never through the relay.

Blurred areas are baked into the image on your computer before anything is uploaded or sent to an AI provider.

## What is stored, and where

- **On this computer only** (`chrome.storage.local` and IndexedDB): open drafts with their screenshots, recordings and recorded page errors, GitHub and issue tracker tokens (including personal API tokens, never synced), API keys, image host keys and the list of issues you created, with the Imgur delete codes of their screenshots.
- **In Chrome sync** (`chrome.storage.sync`, if you have Chrome sync on): non-secret settings, such as AI provider and model, language, image host type and address, Imgur Client-ID, screenshot branch name and default repositories.

Creating an issue clears its draft. Disconnecting a tracker deletes its tokens. A token the tracker no longer accepts is deleted too. Removing the extension deletes everything it stored locally. Options has a button to clear the issue history.

## Permissions

The extension asks for access to other sites only when you need it: an AI provider's API when you save its key, and your image host's address (or `api.imgur.com` for Imgur) when you save it. When you connect an issue tracker with a personal API token, it asks only for access to that tracker's API (`<your site>.atlassian.net` for Jira Cloud, `api.linear.app` and `storage.googleapis.com` for Linear, `app.asana.com` for Asana, `api.trello.com` for Trello). When you connect through the tracker's sign-in page, it asks for `identity`, to open the tracker's sign-in page in a Chrome window, and for access to the sign-in relay and the tracker's API (`api.atlassian.com` and `auth.atlassian.com` for Jira Cloud, `api.linear.app` and `storage.googleapis.com`, where Linear stores uploads, for Linear, `app.asana.com` for Asana; for Trello, access to `api.trello.com` and `trello.com` instead of the relay). It asks for permission to capture tabs (`tabCapture`, with `offscreen` to run the recorder) the first time you record. The `debugger` permission is granted at install, because Chrome doesn't let extensions ask for it later, but the extension uses it only while error recording is on; turning error recording off in Options stops it on every tab.

## Changes and contact

Changes to this policy are published at this address with a new date. For questions, open an issue in the Snap Issue repository or contact the developer through the Chrome Web Store listing.
