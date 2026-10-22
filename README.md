# Auto-Complete & Search Suggestion Engine

An interactive search box powered by a trie (prefix tree). It ranks suggestions by popularity, tolerates typos, learns from what you pick, and shows what the engine is doing behind the scenes.

**Live demo:** https://YOUR-USERNAME.github.io/autocomplete-search-engine/

## Features

- Instant prefix suggestions from a trie, with the typed part shown lighter and the completion in bold
- Popularity ranking, with alphabetical order as the tie-breaker
- Typo tolerance: when few terms match, similar terms are suggested and labeled "similar" (for example, typing "pyhton" still finds Python terms)
- Learning: picking a suggestion raises its score by 25, so it ranks higher next time (can be turned off)
- Empty-box suggestions: your recent searches and the most popular terms
- Full keyboard support: Arrow Up/Down to move, Enter to choose, Escape to close
- Accessible combobox markup (ARIA roles and live status messages)
- Engine internals panel: terms indexed, trie node count, query time, matches found, match type, and the path your query takes through the trie
- Add your own terms to the index
- Adjustable number of suggestions (3, 5 or 8)
- Responsive layout with light and dark themes that follow your system setting

## How it works

### The trie

Every term is stored one character at a time. Words that share a beginning share the same nodes, so `java tutorial` and `java streams` reuse the path for `java `. A node that ends a term holds that term and its score.

To find suggestions for a prefix, the engine walks down the trie one character at a time. If the walk succeeds, it collects every term below that node, sorts them by score and returns the top results. The cost depends on the length of the prefix and the number of matches, not on the total number of indexed terms.

### Typo tolerance

If fewer suggestions than requested are found and the query has at least 3 characters, the engine runs a fuzzy search. It walks the trie while keeping one row of the Levenshtein edit-distance table per node. Branches that exceed the edit budget are pruned, so most of the trie is never visited.

- Queries of 3 to 5 characters allow 1 edit
- Queries of 6 or more characters allow 2 edits

Fuzzy results are ranked by edit distance first, then by score, and are shown after the normal prefix matches.

### Ranking and learning

Each term has a numeric score (its popularity). Suggestions are sorted by score, highest first. When learning is on, choosing a suggestion adds 25 to that term's score. Scores reset when you reload the page.

## Run locally

There is no build step and no dependencies. Download or clone the repo and open `index.html` in a browser.

```bash
git clone https://github.com/YOUR-USERNAME/autocomplete-search-engine.git
cd autocomplete-search-engine
# open index.html in your browser
```

The page loads one Google Font. If you are offline, it falls back to your system font.

## Deploy with GitHub Pages

1. Make sure the main file is named `index.html`.
2. In the repo, go to **Settings > Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select `main` and `/ (root)`, then click **Save**.
4. After a minute or two the site is live at `https://YOUR-USERNAME.github.io/autocomplete-search-engine/`.

## Project structure

```
autocomplete-search-engine/
├── index.html   # the whole app: HTML, CSS and JavaScript
└── README.md
```

## Notes and limits

- The built-in dataset has about 85 sample programming and tech searches. Their scores are made up for the demo.
- Everything runs in the browser. Added terms, scores and recent searches are not saved between visits.
- Queries are lowercased and extra spaces are collapsed before matching.

## Ideas for the future

- Save added terms and learned scores in the browser
- Debounced suggestions from a real search API
- Rank by recency as well as popularity
- Load a custom word list from a file
- Phrase and word-order matching (for example, "tutorial python" finding "python tutorial")

## Built with

Plain HTML, CSS and JavaScript. No frameworks and no build tools.

## License

Add a license of your choice, for example [MIT](https://choosealicense.com/licenses/mit/), by creating a `LICENSE` file in the repo.
