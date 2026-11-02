# CallTrail Privacy Policy

**Effective date:** November 2, 2026

**Applies to:** the CallTrail plugin for JetBrains IDEs, distributed through JetBrains Marketplace and the official [GitHub Releases](https://github.com/0xZZSheep/CallTrail-intellij-plugin/releases) page.

CallTrail is developed by Kai Li ("we", "us", "the developer"). This policy explains what the plugin does with your data. It covers the plugin itself; JetBrains Marketplace and any AI service you configure have their own privacy policies.

## Summary

- CallTrail does not require an account and does not include analytics, advertising, or tracking.
- CallTrail reads your source code locally to build call graphs. The developer does not receive your code, except for anything you choose to include in a crash report.
- AI-assisted summaries are **off by default**. If you enable them, CallTrail sends the selected node's name, location, and source code to the OpenAI-compatible endpoint **you configure**. With a local endpoint such as Ollama or LM Studio, that data stays on your machine.
- Your settings, including the AI API key, are stored locally in your IDE configuration. They are not sent to the developer.
- The only diagnostic data that can reach the developer is a crash report you explicitly choose to send through the IDE's error dialog.

## What CallTrail processes locally

CallTrail uses the IntelliJ Platform's call hierarchy to build a graph of callers and callees for the element you select. The graph is computed inside your IDE from the source code and indexes the IDE already provides. CallTrail does not upload your project.

CallTrail stores its settings locally in your IDE configuration, including graph depth, the node limit, library-node and filter settings, popup behaviour, and AI settings. The application-level settings file is `callHierarchyGraph.xml`; per-project filter rules are stored in the project's IDE configuration. These settings are not sent to the developer. They remain on your machine until you change them or remove the relevant IDE configuration.

## AI-assisted summaries

AI features are **off by default** (`aiEnabled = false`). They require you to enable the feature and configure an OpenAI-compatible endpoint.

When AI is enabled, CallTrail sends the following to the endpoint you configured:

- the selected node's name and location (for example, a method signature and its file and line);
- the source code of the selected node, or of the whole file when a file-level summary is requested;
- the prompt text that asks the model for the summary.

The endpoint may be OpenAI, Azure OpenAI, OpenRouter, Ollama, LM Studio, or another compatible service.

- The developer does not receive your source code, prompts, or API key.
- The AI provider you choose may log or retain requests under its own privacy policy and terms. Review them before enabling AI.
- If you use a local endpoint such as Ollama or LM Studio, requests stay on your machine or local network, depending on how you configure it.
- CallTrail makes no AI requests automatically while the feature is disabled. The **Verify** button in Settings sends a minimal test request only when you click it.
- The API key is stored locally in `callHierarchyGraph.xml` and is sent only to the endpoint you configure, as part of the API request. It is not written to the plugin log. Protect access to your IDE configuration directory.

## Crash and error reports

CallTrail registers the IntelliJ Platform's Marketplace error report handler. If the IDE shows an error dialog and you choose to send a report, the report is submitted to JetBrains Marketplace and may be visible to the developer. It can include the exception stack trace, IDE and plugin versions, operating system, and any details you enter in the dialog. Reports are sent only when you choose to send them.

If you do not send a report, no report is transmitted. JetBrains processes Marketplace data under the [JetBrains Privacy Policy](https://www.jetbrains.com/legal/docs/privacy/privacy.html).

## What CallTrail does not do

- No account or sign-in.
- No analytics, advertising, or tracking.
- No cookies or web beacons.
- No sale or sharing of your data by the developer.
- No background upload of your project or source code.
- No access to files beyond the code and indexes that the IDE exposes for the current project and its dependencies.

## Third parties

- **JetBrains** provides the IDE, JetBrains Marketplace, plugin distribution and update checks, and the optional error-report handler. JetBrains processes data under its own privacy policy.
- **Your AI provider** processes AI requests under its own privacy policy and terms, as described above.

CallTrail does not integrate with any other third-party service.

## Data retention

The developer does not maintain a database of plugin users and does not retain personal data from the plugin. Crash reports are retained by JetBrains, and AI requests are retained by the provider you choose, according to their policies. Settings remain in your IDE configuration until you change or remove them.

## Your rights

Because the developer does not collect personal data through the plugin, there is no developer-held personal data to access, correct, or delete. If you have questions about this policy or believe the plugin has handled data differently, contact us at the address below. For data held by JetBrains or your AI provider, contact that provider.

## Changes

We may update this policy when the plugin's data handling changes. The effective date at the top will be updated, and material changes will be noted in the plugin's release notes. The current version is always available at this URL.

## Contact

Kai Li
Email: likaiqiang.frontend@gmail.com
