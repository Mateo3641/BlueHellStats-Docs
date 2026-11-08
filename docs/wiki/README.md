# 🌐 How to Sync this Wiki to GitHub Wiki

GitHub stores project wikis in a separate Git repository named `<repository-url>.wiki.git`.

### Initial Setup & One-Click Sync

1. Open your repository on GitHub and click on the **Wiki** tab.
2. Create the first dummy page and click **Save** (this initializes the wiki Git repository on GitHub).
3. To push all markdown files from this directory (`docs/wiki/`) directly to your GitHub Wiki, run:

```bash
# Clone or push directly to the wiki remote:
git clone git@github.com:Mateo3641/BlueHellStats.wiki.git /tmp/bhe-wiki
cp -r docs/wiki/*.md /tmp/bhe-wiki/
rm -f /tmp/bhe-wiki/README.md
cd /tmp/bhe-wiki
git add .
git commit -m "docs(wiki): update wiki documentation"
git push origin master
rm -rf /tmp/bhe-wiki
```

Now your GitHub Wiki will have the full sidebar, footer, and all documentation pages beautifully formatted!

