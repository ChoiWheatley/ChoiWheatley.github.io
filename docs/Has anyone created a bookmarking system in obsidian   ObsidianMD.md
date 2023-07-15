---
description:
aliases: 
created: 2023-02-12T12:31:25
tags: []
source: https://www.reddit.com/r/ObsidianMD/comments/we4b06/has_anyone_created_a_bookmarking_system_in/
author: 
date created: Sunday, February 12th 2023, 12:31:25 pm
date modified: Monday, February 27th 2023, 6:20:45 pm
updated: 2023-07-15T21:33:04
title: Has anyone created a bookmarking system in obsidian   ObsidianMD
---

# Has anyone created a bookmarking system in obsidian ??? : ObsidianMD

> ## Excerpt
> 31 votes and 18 comments so far on Reddit

---
Yes, and I don’t just bookmark, I **archive the body text as markdown**.

**Note**: For Android, do the equivalent of what I’m about to suggest. Pointers below MacOS and iOS techniques:

You can clip from inside Obsidian, but that’s an extra friction compared to bookmarking from inside browser. So I use browser extensions.

(A) Bookmarking “System”

Archive every web page you ‘bookmark’ as full markdown page with your own metadata fields.

Key to this approach is what data you use in YAML frontmatter, and setting up your own **Dataview**:

[https://blacksmithgu.github.io/obsidian-dataview/](https://blacksmithgu.github.io/obsidian-dataview/)

MacOS

For **MacOS**, use **MarkDownload** and save the page you want to “bookmark” **as a markdown file** in an Obsidian vault inbox.

For Safari, this is in App Store as Safari extension. For Chrome, Edge, Firefox, see:

[https://forum.obsidian.md/t/markdownload-markdown-web-clipper/173](https://forum.obsidian.md/t/markdownload-markdown-web-clipper/173)

After you have the extension in browser, manage its settings to edit file names to match your system. I use “yyyy-mm-dd - title.md” instead of Zettlekasten, easy to configure, easy to scan later.

iOS

For **iOS**, I set up this Shortcut to grab a URL, download the page and convert to markdown, then save into Obsidian vault inbox:

[https://www.icloud.com/shortcuts/f5fd3c7b46384cd5a6c4669af582ba1a](https://www.icloud.com/shortcuts/f5fd3c7b46384cd5a6c4669af582ba1a)

This Shortcut requires **Toolbox Pro for Shortcuts**:

[https://apps.apple.com/us/app/toolbox-pro-for-shortcuts/id1476205977](https://apps.apple.com/us/app/toolbox-pro-for-shortcuts/id1476205977)

Both are set up to match file names and YAML frontmatter. You can customize those fields for use with Obsidian data.

Android (unverified)

Because you are on **Android** you don’t have Shortcuts built in, and can’t read that. So here is a screenshot of what the Shortcut does, step by step:

[https://i.imgur.com/kDgiNKF.jpg](https://i.imgur.com/kDgiNKF.jpg)

Perhaps you can recreate similar in Macrodroid:

[https://reddit.com/r/ObsidianMD/comments/wd9kps/just\_discovered\_how\_to\_take\_fast\_markdown\_notes/](https://reddit.com/r/ObsidianMD/comments/wd9kps/just_discovered_how_to_take_fast_markdown_notes/)

Otherwise, this thread discusses a few folks’ approaches to trying this on Android, e.g. **Kiwi browser** …

[https://www.reddit.com/r/ObsidianMD/comments/phvrk7/clipping\_web\_pages\_on\_android/](https://www.reddit.com/r/ObsidianMD/comments/phvrk7/clipping_web_pages_on_android/)

Or latest **MarkDownload 3.1.0 release on Firefox Nightly**:

[https://github.com/deathau/markdownload/releases](https://github.com/deathau/markdownload/releases)

(B) Real bookmarking app

You can have the system in Obsidian, or use an existing system that can mirror into Obsidian daily for further use.

For example…

Pinboard + Android + Obsidian

As a completely different approach, bookmark using Pinboard.in:

[https://pinboard.in/](https://pinboard.in/)

See Pinkt for Android for example:

[https://play.google.com/store/apps/details?id=com.fibelatti.pinboard&hl=en\_US&gl=US](https://play.google.com/store/apps/details?id=com.fibelatti.pinboard&hl=en_US&gl=US)

And then bring them in through Obsidian Pinboard plugin:

[https://forum.obsidian.md/t/pinboard-to-obsidian-plugin-available/29412](https://forum.obsidian.md/t/pinboard-to-obsidian-plugin-available/29412)
