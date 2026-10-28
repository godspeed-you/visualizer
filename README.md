# Infra Canvas

A small, standalone infrastructure diagram editor. Just one HTML file — no build step, backend, dependencies, accounts, or external API calls.

**Interface language:** German. **Works offline:** yes, open `index.html` in a modern browser.

## Features

- Layout: tools (**Verbinden**, **＋ Standort**, **＋ Subnetz**) and the palette on the left; undo/redo and file actions in the header; zoom and layer toggles float over the canvas.
- Create, rename, and delete multiple sites, and resize each one with the grip in its lower-right corner (**Größe zurücksetzen** in the inspector restores the default).
- Create subnets as movable and resizable visual groups within a site. Store a subnet name and a CIDR/description label.
- Drag hosts, VMs, containers, firewalls, routers, switches, repeaters, end devices (laptop/PC, smartphone/tablet, IoT device, printer, TV), applications, and storage elements from the palette.
- Nest VMs, containers, firewalls, routers, and switches into a host, and applications or storage/DB into any device (host, VM, container, firewall, router, switch, or an end device); nested elements move with their container. Reorder the entries inside a container by dragging them to a new position (a blue line marks the insertion point) or with **↑ Nach oben / ↓ Nach unten** in the inspector.
- Create one or more subnets inside a host (inspector: **＋ Subnetz im Host**) and drop VMs/containers into them. Each host subnet can name a firewall or router as its **gateway**.
- Widen a selected host (or any other top-level container) with the grip on its right edge. Once it is wide enough, its VMs, containers and services are laid out in two or more columns, also inside host subnets.
- Mark any element as **Inaktiv** (inspector, field **Status**). It is drawn pale with a dashed outline and an INAKTIV tag; everything nested inside an inactive container and the connections touching it are drawn pale as well. Stored as `inactive: true`.
- Record any number of IP addresses per element, each with an optional label (e.g. `eth0`, `LAN`). The board shows the first address plus a count; hovering an element lists all of them.
- Record how a hypervisor is reachable (**Erreichbar über**: a site or host subnet, plus an optional management IP).
- **Group** free-standing elements: select them with Shift/Ctrl/⌘-click and choose **Gruppieren**, or pick a group in the inspector field **Gruppe** (also on touch devices). A group aligns its elements in a column, a row, or a 2-/3-column grid, is moved by its header, and can be named. Drag an element out of the frame to remove it, into a frame to add it; Entf or **Gruppe auflösen** keeps the elements in place.
- Assign nodes to subnets by dropping them inside the visual group or choosing a subnet in the inspector.
- Connect nodes and edit connection labels. A connection has a kind, preset from its ends and changeable in the inspector:
  - **Physisch (Kabel)** – solid; ports at switches/routers, speed (thicker from 10 Gbit/s), medium (copper, fibre, DAC, virtual), VLANs.
  - **WLAN (Funk)** – dotted teal; SSID, frequency band, Wi-Fi standard. Preset for a repeater and its clients, and for smartphones/IoT devices at a router.
  - **Tunnel / VPN** – dashed; protocol (IPsec, WireGuard, …).
  - **Dienst (Abhängigkeit)** – thin violet line with an arrow for the direction; protocol/port and purpose.
- Show or hide each kind with the layer toggles at the bottom right of the canvas (also applies to exports; a view setting, not stored in the diagram). Several lines at one side of a card get separate anchor points.
- Merge two or more connections that lead to the same element (e.g. several services using one database): choose **Zusammenführen** on a connection, or **Verbindungen hier zusammenführen** on the shared element. Connections merged at the same side with the same line style join at a junction point and continue as one line.
- Give switches, routers and repeaters a port count; for a connection to a switch or router, choose the port it uses (occupied ports are disabled). The board labels the line end with the port, the card shows the usage (e.g. `3/24 Ports`), and the inspector lists which device is on which port.
- Zoom in and out (controls at the bottom right of the canvas, Ctrl/⌘ + mouse wheel or pinch, Ctrl/⌘ + `+`/`-`/`0`); by default the board fits the visible area. Zooming scales only the content inside the board; drag the empty background to pan.
- Works on smartphones and tablets: on narrow screens the header actions move into a **⋯** menu, the palette becomes a swipeable strip, and the inspector opens as a bottom sheet. Touch gestures: tap to select (or to add a palette element), drag with one finger to pan, pinch with two fingers to zoom, long-press to move a node, subnet, or palette element.
- Undo/redo, auto-save in the browser, JSON import/export, and SVG/PNG image export.
- Import diagrams from older Infra Canvas v1/v2 versions.

## Quick start

1. Open `index.html` in a browser. That's it.
2. Click **Neu** to start with a blank document, or explore the built-in **Demo** (fictional example data only).
3. Select a site and click **+ Subnetz**. Drag the subnet by its header; resize using its lower-right corner.
4. Drop nodes into a subnet. Select any node or subnet to edit it in the inspector, which slides in from the right while something is selected (close it with ✕, Esc, or a click on empty canvas).
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
- Nesting follows fixed rules (`CONTAINS` in `index.html`): host → VM/LXC/firewall/router/switch/application/storage, VM/LXC/firewall/router/switch/repeater/laptop/mobile/IoT/printer/TV → application/storage. A connection stores its kind as `kind` (`physical`, `wifi`, `tunnel`, `service`) with `speed`, `medium`, `vlan`, `ssid`, `band`, `wifiStd`, `protocol`, `dir`, `purpose` as applicable; cables with the former medium `wireless` load as WLAN; `style` is still written for older versions. Files without `kind` are classified on load: VPN style → tunnel, a connection touching an application or storage → dependency, anything else → cable. A merged connection stores the shared end as `bundle` (`"from"` or `"to"`). Port data is optional too: `ports` on a switch/router node, `fromPort`/`toPort` on a link (port of the switch/router at that end). Groups are stored in `groups` (`id`, `site`, `name`, `layout`, `x`, `y`) with `groupId` on member nodes; member positions are derived and also saved, so older versions show them in place. IP addresses are an optional `ips` list per node (`[{"addr": "10.0.0.5/24", "label": "eth0"}]`, plain strings are accepted on import); diagrams without it load unchanged. Like CIDR labels, addresses are free text and are not validated. Guests of a host can be grouped into host subnets (`nets` on the host, `netId` on the guest). The layout inside containers is automatic (one column per 200 units of width, row by row in document order); optional sizes are stored as `w`/`h` on a site and `w` on a container node and are ignored by older versions; arbitrarily deep nesting is not supported.
- The interface is primarily designed for desktop mouse/pointer interaction. Touch input on the board uses its own gesture handling (`onBoardDown`/`onBoardMove`/`onBoardUp` in `index.html`); mouse and pen keep the direct desktop behaviour. Complex layout work is still easier on a desktop.

Issues and pull requests are welcome; please keep the application dependency-free unless there's a compelling reason to change that goal.

## License

MIT. See [`LICENSE`](LICENSE). Review the copyright attribution before publishing under your own GitHub account.
