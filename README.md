# claude-eval-harness

Run the same prompt against multiple Claude models side-by-side. Compare outputs, latency, and cost. BYOK. Single HTML file.

## Why

Picking the right Claude model for a task is a real decision: Opus is smartest, Haiku is cheapest and fastest, Sonnet is the middle. Most people pick one and stick with it without testing whether they could downshift to a cheaper tier with no quality loss.

This tool removes the friction. Paste a prompt, pick 2-4 models, click Run. See the outputs in a grid. See actual latency in milliseconds. See real token counts and computed cost per call.

For prompt iteration, this is also useful: try a prompt variation against the same models and watch how output quality and length shift.

## Use it

Open `index.html` in any modern browser. Or visit the hosted version at `https://0xelitesystem.github.io/claude-eval-harness/` once GitHub Pages is enabled.

### Workflow

1. Paste your Anthropic API key. The tool validates it with a 1-token test request.
2. Type a prompt.
3. Pick which models to test. Defaults are Sonnet 4.6 and Haiku 4.5.
4. Optional: open Advanced Options for system prompt and max-tokens.
5. Click Run all selected. Requests fire in parallel; cells update as each one returns.
6. Below the grid, an aggregate row shows total cost, fastest model, and cheapest model.
7. Export JSON saves the entire session for later comparison or sharing.

## Pricing accuracy

The tool ships with hardcoded pricing per model (input / output USD per 1M tokens). These reflect publicly listed rates at time of build. Verify current pricing at [anthropic.com/pricing](https://www.anthropic.com/pricing) before relying on the cost numbers for billing decisions.

To update pricing, edit the `MODELS` array at the top of the script block in `index.html`. It is the only place pricing values appear.

Costs shown are computed estimates, not invoices. Actual billing comes from your Anthropic account.

## Models supported out of the box

- Claude Opus 4.7 (Frontier)
- Claude Opus 4.6 (Frontier)
- Claude Sonnet 4.6 (Balanced)
- Claude Haiku 4.5 (Fast)

To add more models or remove some, edit the `MODELS` array. Each entry is: `{id, name, tier, inputPer1M, outputPer1M, contextWindow}`. The `id` must match an Anthropic model identifier. The other fields are display-only.

## Security

- API key stays in a JavaScript variable for the session. Never written to localStorage, sessionStorage, cookies, or any URL.
- Sent only to `api.anthropic.com` over HTTPS. Verifiable in DevTools Network tab.
- No third-party scripts. No analytics. No fonts. No CDN libraries. The whole tool is one file.
- Closing the tab, refreshing the page, or clicking Clear key wipes the variable.

This pattern follows [byok-patterns/browser-byok](https://github.com/0xelitesystem/byok-patterns/tree/main/browser-byok). The known weakness is XSS in your own code; mitigations and a deeper discussion are in that repo.

## Hosting your own

Push this repo and enable GitHub Pages:

1. Settings → Pages
2. Source: Deploy from a branch
3. Branch: main, folder /(root)
4. Save. URL appears at the top.

That's it. Free hosting, your domain, BYOK so users pay their own API costs.

## What it doesn't do

- Streaming responses. Responses come in one shot per call. Good enough for evaluation.
- Multi-turn conversations. Single prompt only.
- Side-by-side diff highlighting. The grid layout makes outputs comparable visually; future versions may add a diff toggle.
- Cross-provider comparison. Anthropic-only. For multi-provider, see `byok-patterns/multi-provider` (planned).

## Tech

- Single HTML file
- ~700 lines including CSS and JS
- Vanilla JS, no frameworks, no dependencies, no build
- Tested in current Chrome, Firefox, Safari

## License

MIT. See [LICENSE](LICENSE).

## Related

- [prompt-templates](https://github.com/0xelitesystem/prompt-templates) — production prompts targeting LLM failure modes
- [byok-patterns](https://github.com/0xelitesystem/byok-patterns) — BYOK reference implementations
- [readme-slop-checker](https://github.com/0xelitesystem/readme-slop-checker) — audit a README for AI cliches
- [claude-skills-templates](https://github.com/0xelitesystem/claude-skills-templates) — five reference Claude Skill patterns
