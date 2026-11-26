<h1><img src="images/agent-relay.png" width="36" height="36" alt="" align="middle"> Agent Relay</h1>

**Keep working in Codex while Gemini handles a separate task.** Agent Relay runs the handoff on Windows and shows how it’s going. When the work is done, Codex can pick up the report in the same chat, check the changes, and decide what belongs in the project.

**[Download Agent Relay for Windows](https://github.com/korzhy/agent-relay-downloads/releases/latest/download/AgentRelaySetup-x64.exe)** · Windows 10/11 x64 · .NET included

[Русская версия](README.ru.md) · [English guide](docs/guide.md) · [Руководство на русском](docs/guide.ru.md) · [What’s new](https://github.com/korzhy/agent-relay-downloads/releases/latest)

**You can leave the Antigravity desktop app closed.** Install Antigravity CLI (`agy`) and connect a Google account with model access. Relay starts the CLI when needed. [Set up Relay and try a task](docs/guide.md#set-up-and-try-a-task) · [Verify the installer](docs/guide.md#verify-the-installer)

## How it fits your work

- **Give Hydrogen a complete piece of work.** Say you’re fixing a UI bug. Hydrogen High can add a new CSV field to the import and its validation, with checks for older files, while Codex continues the UI fix. Set the delegation threshold to **Баланс** (Balance) or **Активно** (Active) so Codex considers jobs like this when it can verify the result.
- **Choose the executor and its effort.** Relay starts with **Hydrogen High**. **Flash High** is also available. In 0.5.22, choose **Авто** (Auto) for Hydrogen to let Codex pick Low, Medium, or High for each task. The delegation threshold controls when Codex considers a handoff. Effort controls how much reasoning Hydrogen uses on that task.
- **Run two independent tasks at once.** Optional Paired mode adds a second Windows user. Each executor gets a separate working copy, so both can work on tasks from the same repository. Codex checks each result before integrating it.

![Agent Relay shows a Gemini report ready for Codex to pick up and review](images/report-ready.png)

*A sample task in Relay 0.5.21. Gemini has finished and the report is ready for Codex.*

## Updates and help

- **Update in Relay.** When the update chip appears, click **Обновить** (Update). Relay checks the download, installs it, and restarts. If a task is still running, try again when it finishes. [Manual updates](docs/guide.md#update-relay)
- **Check what’s missing.** Open **Диагностика** (Diagnostics) from the Relay status button to check Codex, the CLI, and model access. [Troubleshooting](docs/guide.md#troubleshooting)
