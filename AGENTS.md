# AGENTS.md - Maintenance Guide for AI Agents

This file documents the recurring maintenance tasks that need to be performed when content is added to the website. Follow this checklist every time a tutorial, plugin, or app is added.

## Card Stats on Landing Page (index.html)

The landing page at `index.html` displays a stats count on each project card. These counts **must be kept in sync** with the actual content on each subpage.

### Current Mapping (as of last update)

| Card on index.html | Linked page | Stat shown | How to count |
|---|---|---|---|
| Mobile Development | `myapps.html` | 1 app available | Count of `id="guess-sequence"` |
| Grasshopper Plugins | `grasshopper-plugins.html` | 2 plugins | Count of `class="plugin"` |
| Grasshopper Tutorials | `grasshopper-tutorials.html` | 20 tutorials | Count of `class="tutorial"` |
| Rhino Plugins | `rhino-plugins.html` | 1 plugin | Count of `class="plugin"` |
| 3DS Max Plugins | `3dsmax-plugins.html` | 11 plugins | Count of `class="plugin"` |
| 3DS Max Tutorials | `3dsmax-tutorials.html` | 7 tutorials | Count of `class="tutorial"` |

### PowerShell Command to Recount

Run this from the website root to get fresh counts after adding new content:

```powershell
$files = @{
  "grasshopper-plugins" = "grasshopper-plugins.html"
  "grasshopper-tutorials" = "grasshopper-tutorials.html"
  "rhino-plugins" = "rhino-plugins.html"
  "3dsmax-plugins" = "3dsmax-plugins.html"
  "3dsmax-tutorials" = "3dsmax-tutorials.html"
  "myapps" = "myapps.html"
}
foreach ($key in $files.Keys) {
  $content = Get-Content $files[$key]
  $pluginCount = ([regex]::Matches($content, 'class="plugin"')).Count
  $tutorialCount = ([regex]::Matches($content, 'class="tutorial"')).Count
  $appCount = ([regex]::Matches($content, 'id="guess-sequence"')).Count
  Write-Output "$key : plugins=$pluginCount tutorials=$tutorialCount apps=$appCount"
}
```

### Workflow When Adding a New Tutorial/Plugin

**Step 1**: Add the new tutorial/plugin to its respective page (e.g., `3dsmax-tutorials.html`).
Follow the existing pattern:
- Add a `<li>` to the Quick Navigation `<ul>` in the `.nav-menu`
- Add a new `<div class="tutorial" id="...">` block with the video, description, and "What You'll Learn" section

**Step 2**: Update the corresponding stat on the landing page (`index.html`).
- Find the matching `.project-card` for that page
- Update the `<p class="card-stats">` text to the new count

**Step 3**: Update the `<lastmod>` in `sitemap_4567513.xml` for the changed page to today's date/time.

**Step 4**: (Only if you created a brand new HTML page) Add a new `<url>` entry to `sitemap_4567513.xml`.

## Sitemap Maintenance

- **Adding a tutorial/plugin to an existing page**: Just update that page's `<lastmod>` in the sitemap
- **Creating a new page**: Add a new `<url>` entry with the current date/time as `<lastmod>`

## Style Consistency

When adding new pages, follow the established patterns from the 6 existing pages under "Explore My Work":
- Use `<nav>` with `<a href="index.html">Home</a>` then `<a href="about.html">About</a>` (Home first)
- Use a `.nav-menu` block with Quick Navigation
- Use `.tutorial` (for tutorials) or `.plugin` (for plugins) class for content cards
- Use `.video-container` with the 16:9 responsive iframe pattern
- Include the floating `.back-to-top` button before `</body>` (CSS, HTML, and JS)
- Use the same color scheme: `#007bff` (blue primary), `#28a745` (tutorial green), `#6f42c1` (app purple)
- Use the same style.css file (don't create duplicate styles)
