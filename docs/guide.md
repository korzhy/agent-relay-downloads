# Agent Relay quick start

[Overview](../README.md) · [Руководство на русском](guide.ru.md) · [Latest release](https://github.com/korzhy/agent-relay-downloads/releases/latest)

## Before you install

- **Windows 10 or 11, x64.** The Relay installer includes .NET and installs for the current Windows user without administrator rights.
- **OpenAI Codex desktop app.** Install it and sign in under that Windows user. Open the project in Codex to start a handoff. Relay uses the open Codex chat. [OpenAI’s Windows setup guide](https://learn.chatgpt.com/docs/windows/windows-app#download-the-chatgpt-desktop-app).
- **Google Antigravity CLI (`agy`).** Install it separately under the same Windows user. You need a Google account with access to a supported model and available quota. Relay reads usage from `agy`. Check your plan and model access with Google. [Google’s Antigravity CLI setup](https://antigravity.google/docs/getting-started?tab=cli).
- **A practice repository.** Use a Git repository with dummy data for the first run, so Codex can inspect the diff and you can safely discard trial changes.

## Verify the installer

Open the [latest release](https://github.com/korzhy/agent-relay-downloads/releases/latest) and download both `AgentRelaySetup-x64.exe` and `AgentRelaySetup-x64.exe.sha256` from that same release. The [direct download](https://github.com/korzhy/agent-relay-downloads/releases/latest/download/AgentRelaySetup-x64.exe) is convenient when you only need the installer.

In File Explorer, open the folder containing both files. Type `powershell` in the address bar and press Enter, then run:

```powershell
$expected = ((Get-Content -LiteralPath '.\AgentRelaySetup-x64.exe.sha256' -Raw).Trim() -split '\s+')[0]
$actual = (Get-FileHash -LiteralPath '.\AgentRelaySetup-x64.exe' -Algorithm SHA256).Hash
if ($expected -match '\A[0-9a-f]{64}\z' -and $actual -eq $expected) {
    'SHA-256 matches'
} else {
    'SHA-256 mismatch. Do not run the installer.'
}
```

The installer currently has no Windows publisher signature. Windows may show an unknown-publisher or SmartScreen warning. A matching SHA-256 confirms the downloaded bytes match the checksum published on GitHub. SHA-256 cannot establish the publisher’s identity or replace a digital signature.

## Set up and try a task

1. **Install and open Relay.** Run the verified installer as your regular Windows user. Setup installs the managed Codex instructions and skill for that user. Open Agent Relay from Start.
2. **Connect Antigravity.** Open **Аккаунты** (Accounts), enter a local label, and use **Добавить аккаунт** (Add account). Finish Google sign-in in the browser and make the account active in Relay. Click the Relay status button to open **Диагностика** (Diagnostics) and check **Readiness**.
3. **Set the threshold.** Use **Баланс** (Balanced) for the trial and **Обычный** (Normal) for one executor. **Выкл.** disables external execution. **Точечно** favors clear mechanical tasks. **Активно** lets Codex consider more verifiable tasks. This controls how readily Codex delegates, independently of its model or reasoning effort.
4. **Open a new Codex chat in the practice repository.** Give it one narrow task and a check it can run itself. If Codex says Relay requires trust, check the exact path and authorize Codex to grant trust for that folder. Keep workspace Auto-approve off for the trial. That setting grants trust to eligible new workspaces without a per-folder confirmation, and turning it off leaves earlier grants in place.
5. **Return to that same chat for the result.** When Relay shows a report is ready, ask Codex to pick it up from the Relay inbox, check the diff, and run the agreed checks before integrating it.

For a first task, commit three shuffled lists of lowercase sample names, one name per line, in a `samples` folder. Try this in Codex:

> Assess whether to use Agent Relay to sort each text file in `samples/` alphabetically. Allow edits only to those files. If you delegate, retrieve the report in this chat and independently check that each list is sorted and contains exactly the same entries as before.

Codex may still do the task itself if a handoff would take more work.

### Choose the executor model

From 0.5.21, Relay defaults to **Hydrogen High** and preserves an explicit saved choice. In **Исполнитель** (Executor), you can choose **Flash High** or select Hydrogen with an effort available in your Antigravity catalog. Click **Сохранить выбор** (Save selection) to apply it to new tasks in either mode.

## Paired mode and local data

- **Парный** (Paired) is optional. Use **Настроить второго** (Set up second executor) to configure a second existing standard Windows user. Both profiles need Relay installations of the same build and working Antigravity access. Explicit permission is required to create derived copies of already trusted workspaces. Codex uses separate copies for independent tasks and integrates results in sequence. From 0.5.21, a finished report frees its executor slot while it waits for review.
- Relay stores settings, workspace trust, handoff records, reports, and diagnostics locally on the PC. Google account credentials managed by Relay are stored in Windows Credential Manager. The task and the source it needs are shared with the Antigravity executor when you delegate. Review what the task allows it to read or edit before approving.

## Update Relay

On 0.5.19 and later, available updates are offered in the main window. Click **Обновить** (Update) to download, verify, install, and restart Relay. The update check alone never installs an update. If Relay reports an active task, wait for it to finish and click again.

For a manual update, run the verified latest installer under the same Windows user. This also works for builds through 0.5.4, which use an older update location. If you use Paired mode, update both profiles to the same build. Start a new Codex chat after updating or repairing the integration so it loads the refreshed instructions.

## Troubleshooting

| Symptom | What to check |
| --- | --- |
| Relay cannot find Codex or `agy` | In **Диагностика**, check **Readiness**. Install both tools under the Windows user running Relay. Open Codex and confirm that `agy --version` runs in a new terminal. |
| Codex never considers a handoff | Start a new chat. Check that delegation is enabled, the workspace is trusted, and the task has an explicit scope and check. If integration is missing, click **Восстановить интеграцию Codex** (Repair Codex integration) in Diagnostics and start a new chat. |
| An account cannot run a task | In **Аккаунты**, check the active account and quota. Check that `agy models` can list a suitable model. |
| A report is ready while Codex is idle | Return to the chat that started the task and ask it to pick up the Relay inbox result. Then check the result before integrating it. |
| Paired mode keeps reporting that it is busy on 0.5.19 | Wait briefly and ask Codex to recheck the same task. If the status window keeps getting in the way, use **Exit** from the Relay tray menu, then reopen Relay after Codex finishes the status check. Exiting the window leaves running executors active. |
| An attempt ends without a validated report | Ask Codex to inspect the current attempt and any partial changes before retrying. For version-specific limitations, see the [release notes](https://github.com/korzhy/agent-relay-downloads/releases). |

The download repository currently has no public support tracker. If you already have a way to contact the maintainer, include the Relay version, Windows version, the step that failed, and a short redacted error message. Keep account details, local paths, repository contents, and full logs private.
