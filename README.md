# CallTrail

**English** · [简体中文](README.zh-CN.md)

**CallTrail** is a **call graph** and **call hierarchy** visualizer for IntelliJ IDEA, WebStorm, PyCharm, GoLand, PhpStorm, RubyMine, CLion, Rider, and Android Studio. Right-click a function or method and choose *Call Graph…* to see up to three caller levels above and three callee levels below it in a popup that stays beside your code instead of taking over a tool window.

The callers and callees displayed in the graph match what appears in *Navigate | Call Hierarchy*, ensuring full consistency with the IDE's built-in analysis.

## Opening the Call Graph

Right-click the element in the editor and choose **Call Graph…** from the context menu. The popup opens at the mouse click location.

## How to Use the Graph

- Callers are rendered above the root element, and callees are rendered below, with each row labeled by its depth level. A recursive node is shown only once, at its first occurrence: the recursion edge points back to that node, which is labeled `Recursion` and outlined with a dashed border. Traversal stops there in the recursive direction so the same cycle is not expanded level after level.
- **Single-click** a node to focus on it—its neighbors remain highlighted while the rest of the graph dims.
- **Right-click** a node to execute `Jump to Source`, `Copy Element Name`, `Copy as Mermaid Diagram`, or `Copy as DOT Diagram`. `Jump to Source` (or pressing <kbd>Enter</kbd> on a selected node) keeps the popup open, allowing you to navigate through multiple nodes consecutively.
- Copied graphs are plain text that can be pasted directly into Markdown documents, PR descriptions, or Graphviz tools. Row alignments across levels are preserved to match the visual layout in the popup.
- To explore deeper than the configured depth limit, jump to the node of interest and open the call graph again on it.
- The popup floats beside your cursor or click location. Since a single node occupies roughly one-fifth of the popup width, large hierarchies will scroll rather than shrink the entire graph: use **Scroll** to pan vertically, <kbd>Shift</kbd>+**Scroll** to pan horizontally, <kbd>Ctrl</kbd>+**Scroll** to zoom, drag to pan freely, and click `Fit` to scale the graph within the view.
- The toolbar features `-`/`+` zoom controls, `Fit`, settings, a **Maximize** toggle to fit the popup to your screen (press again to restore), and a close button. Press <kbd>R</kbd> to recompute the call graph.
- `Center Selected` brings the selected node back to the middle of the visible graph, falling back to the root when nothing is selected. `Filter` can hide file nodes or exclude project folders such as `test`, `src/test` and `**/generated/**`; the rules are saved for the project, and applying them runs the same recomputation as <kbd>R</kbd>.
- Clicking outside the popup will not close it, allowing you to keep it open next to your editor or drag it to another monitor. Close it using the ✕ button in the toolbar or press <kbd>Esc</kbd>. The *Close the graph popup when clicking outside it* setting can restore click-to-close behavior.

## Second Screen and Material Theme

The popup is a real window, not an overlay inside the editor: drag it by its title bar onto a second monitor and it stays there while you keep working in the IDE. Not closing the popup when you click outside of it is what makes that possible.

**Material Theme UI** conflicts with this behavior. Its *Show Overlays* feature paints a translucent overlay over the whole IDE while one of its overlays is on, and the IDE therefore looks dimmed. That overlay sits on the IDE's glass pane, so it swallows the mouse clicks as well: the editor stops reacting to clicks and <kbd>Ctrl</kbd>+Click no longer navigates until the popup is closed — which a popup that is meant to stay open does not survive.

CallTrail therefore turns the two overlay switches of the theme off before the popup is on the screen, and puts them back the way it found them when the popup closes: *Turn off Material Theme overlays while the graph popup is open* in *Settings | Tools | CallTrail*, on by default. The theme is reached through its own settings, so an IDE without it is left untouched. Should the workaround be switched off, or should an overlay be left behind anyway — the theme has a **Clean Overlay** action of its own for that — the switches can be turned off in the theme: open *Settings | Appearance | Material Theme* and uncheck **Show Overlays** in the wizard (or in the **Features** page), or press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>A</kbd> and run **Toggle Overlays** or **Toggle More Overlays**.

## Progress Tracking

The footer of the popup displays live telemetry during graph construction: a progress bar showing completed workload, pending queue items, total discovered nodes, and processing throughput (items/sec). This lets you observe convergence on complex hierarchies instead of staring at a static spinner; queue counts indicate whether the traversal is expanding or draining.

## Settings

Configure options via *Settings | Tools | CallTrail*:

- Max caller and callee depth levels. Until you customize them, Ruby defaults to 2 and 2, while other languages default to 3 and 3. In an IDE with Ruby support, increasing either Ruby value above 2 shows a non-blocking performance warning;
- Node limit per graph (default: 250);
- Include external library elements (JDK, dependencies, `node_modules`, etc.). Disabled by default to restrict graphs to project sources and significantly speed up caller traversals in JavaScript projects. The root element is always displayed;
- Toggle closing the popup when clicking outside;
- Turn off the overlays of the Material Theme UI while the graph popup is open (on by default).

## AI Summaries

Selecting a node opens a persistent, resizable details panel. A concise purpose summary is formatted as a documentation comment above a read-only copy of the node source in the platform editor, with sparse AI explanations placed directly after the lines they describe. The panel width is remembered, source is syntax-highlighted with line numbers, and long source lines scroll horizontally while documentation comments wrap. Parameter, return-value and implementation-detail sections have been replaced by this source-local view.

- When AI-assisted understanding is enabled, **Panorama** provides a short whole-graph overview and a semantic directory. It can be generated automatically on graph open or on demand from the popup toolbar; automatic generation is off by default. Selecting a directory section highlights its real subgraph and exposes related nodes as source evidence; **Show full graph** clears the focus.
- Clicking a graph node, or a node in the panorama's evidence list, opens **Node Details** with the node's AI documentation comment and inline source explanations.
- Summaries utilize your choice of conversational API via any OpenAI-compatible endpoint. Configure the Base URL, API Key, and Model in settings (defaults: `https://api.openai.com/v1` and `gpt-4o-mini`). The adjacent `Verify` button runs a minimal request to validate the configuration. Summary output language is fully configurable.
- Upon opening a graph, the root element and its direct neighbors are prepared immediately. Deeper nodes are prepared when their details are opened by a click; hovering only shows the short identity tooltip and does not request AI content.
- Source code of target nodes is transmitted exclusively to your configured endpoint for summary generation. No other data leaves your IDE, and zero requests are issued when the feature is disabled.
- API keys are stored in IDE settings (`callHierarchyGraph.xml`) and are never written to the plugin log.

## Requirements & Language Support

CallTrail runs on any IntelliJ-based IDE running version **2025.2 or newer** (including IntelliJ IDEA, WebStorm, PyCharm, GoLand, PhpStorm, RubyMine, etc.). The plugin relies strictly on standard IDE APIs without product-specific locks.

CallTrail uses two modes to resolve call graphs depending on the target language:

- **Fast Mode**
  - **Supported Languages**: Java, Kotlin, JavaScript, TypeScript, Python, PHP, Go, Ruby.
  - **Details**: Uses each language's optimized call hierarchy for fast graph construction. Elements the hierarchy cannot walk are resolved through usage searches (*Find Usages*).
- **Compatibility Mode**
  - **Supported Languages**: C/C++, Rust, C#, and any other IDE-supported language.
  - **Details**: Uses the IDE's language call hierarchy with usage-search fallback. Rider C# uses a dedicated integration with performance close to Fast Mode.

For building, testing, and debugging the plugin, refer to [CONTRIBUTING.md](CONTRIBUTING.md).

## License

This project is licensed under the [MIT License](LICENSE) and was created using the [IntelliJ Platform Plugin Template][gh:template].

[gh:template]: https://github.com/JetBrains/intellij-platform-plugin-template
