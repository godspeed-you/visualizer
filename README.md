# Infra Canvas

A small, standalone infrastructure diagram editor. Just one HTML file — no build step, backend, dependencies, accounts, or external API calls.

**Interface language:** German. **Works offline:** yes, open `index.html` in a modern browser.

## Features

- Create, rename, and delete multiple sites.
- Create subnets as movable and resizable visual groups within a site. Store a subnet name and a CIDR/description label.
- Drag hosts, VMs, containers, firewalls, routers, applications, and storage elements from the palette.
- Nest VMs and containers into a host; move hosted guests with their host.
- Assign nodes to subnets by dropping them inside the visual group or choosing a subnet in the inspector.
- Connect nodes, including dashed tunnel links; edit connection labels.
- Undo/redo, auto-save in the browser, JSON import/export, and SVG/PNG image export.
- Import diagrams from older Infra Canvas v1/v2 versions.

## Quick start

1. Open `index.html` in a browser. That's it.
2. Click **Neu** to start with a blank document, or explore the built-in **Demo** (fictional example data only).
3. Select a site and click **+ Subnetz**. Drag the subnet by its header; resize using its lower-right corner.
4. Drop nodes into a subnet. Select any node or subnet to edit it in the right-side inspector.
5. Save an editable copy with **JSON**, or export a static graphic with **SVG** or **PNG**.

## Data and privacy

- The application runs entirely locally and does not make network requests.
- Autosave uses the browser's `localStorage`, under `infra-canvas-public-v3`.
- Data from older, private installations is **not** loaded automatically. Older diagrams can be imported explicitly via **Öffnen**.
- The bundled demo contains only fictional site and component names, with illustrative RFC 1918 network ranges.
- **Do not commit your private JSON diagrams or exported images to a public repository.** The `.gitignore` excludes some conventional private-data locations, but it cannot detect every sensitive file.
- Export your JSON periodically; browser storage is not a durable backup.

## Project structure

- `index.html` — the complete editor (HTML, CSS, vanilla JavaScript, inline SVG).
- `README.md` — usage, privacy, and contribution notes.
- `LICENSE` — MIT license.
- `.gitignore` — common local and private export paths.

## Development

Edit `index.html` directly and reload the page. There is no bundler or dependency installation.

Some design constraints are intentional:
- A subnet is a **visual/documentation grouping**, not a live network scan, VLAN configuration, or CIDR validation engine.
- Site and subnet positions are stored as part of the editable JSON document.
- Only hosts can contain VMs or containers. Arbitrarily deep nesting is not supported.
- The interface is optimized for desktop mouse/pointer interaction.

Issues and pull requests are welcome; please keep the application dependency-free unless there's a compelling reason to change that goal.

## License

MIT. See [`LICENSE`](LICENSE). Review the copyright attribution before publishing under your own GitHub account.
