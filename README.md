# gmux — documentation site

This repo is the **source for the gmux documentation site**, published via GitHub Pages at:

→ **<https://gzhajimi.github.io/gmux/>**

Visit the site for install instructions, the full user guide, FAQ, and privacy policy.

---

## What is gmux?

A declarative workspace switcher for Windows. Describe multi-monitor layouts in a TOML file; press `Ctrl+M` + a key to snap to any scene in 0.3 s. Apps already running are *moved* — not closed, not relaunched.

声明式工作场景切换器（Windows）。把多显示器布局写成 TOML，按 `Ctrl+M` + 一个键 0.3 秒切到任意场景；已开着的应用**直接复用**，不关、不重启。

The product itself, install channels, and full configuration reference live on the site linked above.

---

## Repo layout

| Path | Purpose |
|---|---|
| `index.md` | Site homepage |
| `settings-help-{zh,en,ja}.md` | Five core concepts, every built-in hotkey, settings-window walkthrough |
| `hotkeys/<chord>-{zh,en,ja}.md` | Per-hotkey deep dives |
| `faq-{zh,en}.md` | Common questions and troubleshooting |
| `privacy-{zh,en}.md` | Privacy policy |
| `_config.yml` | Jekyll config (theme: `minima`) |

---

## Contributing to the docs

Found a typo, broken link, or unclear sentence? PRs welcome — every page is plain Markdown.

Bug reports and feature requests for **the app itself** also go to [Issues](https://github.com/gzhajimi/gmux/issues); there is no separate tracker.

---

## License

The site content in this repository is released under the [MIT License](LICENSE) (defaults to MIT if no `LICENSE` file is present). The gmux binary is distributed under its respective Microsoft Store and GitHub Release terms.
