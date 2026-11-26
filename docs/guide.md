# Agent Relay quick start

[Overview](../README.md) · [Руководство на русском](guide.ru.md) · [Latest release](https://github.com/korzhy/agent-relay-downloads/releases/latest)

## Before you install

- **Windows 10 or 11, x64.** The Relay installer includes .NET and installs for the current Windows user without administrator rights.
- **Codex on Windows.** Sign in to the OpenAI desktop app and open Codex. Start your first task from a chat in your project. [OpenAI’s Windows setup guide](https://learn.chatgpt.com/docs/windows/windows-app#download-the-chatgpt-desktop-app)
- **Antigravity CLI (`agy`).** Install it for the same Windows user and use a Google account with model access and available quota. You can leave the Antigravity desktop app closed. [Google’s Antigravity CLI setup](https://antigravity.google/docs/getting-started?tab=cli)
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

1. **Install and open Relay.** Run the verified installer as your regular Windows user and open Agent Relay from Start. Open a new Codex chat so it can read the instructions Relay installed.
2. **Connect your Google account.** In Relay, open **Аккаунты** (Accounts), enter a label, and click **Добавить аккаунт** (Add account). Finish signing in from your browser and make the account active in Relay. The Relay status button opens **Диагностика** (Diagnostics), where you can check that both tools are ready.
3. **Decide when to hand work off.** Start in **Обычный** (Normal) mode with one executor. **Точечно** is for clear mechanical jobs. **Баланс** (Balance) also lets Codex consider larger, self-contained jobs with clear checks. **Активно** (Active) asks it to look for more opportunities to delegate. **Выкл.** turns external execution off.
4. **Open a new Codex chat in the practice repository.** Give it one narrow task and a check it can run itself. If Codex says Relay requires trust, check the exact path and authorize Codex to grant trust for that folder. Keep workspace Auto-approve off for the trial. That setting grants trust to eligible new workspaces without a per-folder confirmation, and turning it off leaves earlier grants in place.
5. **Return to that same chat for the result.** When Relay shows a report is ready, ask Codex to pick it up from the Relay inbox, check the diff, and run the agreed checks before integrating it.

For a first task, commit three shuffled lists of lowercase sample names, one name per line, in a `samples` folder. Try this in Codex:

> Assess whether to use Agent Relay to sort each text file in `samples/` alphabetically. Allow edits only to those files. If you delegate, retrieve the report in this chat and independently check that each list is sorted and contains exactly the same entries as before.

Codex may still do the task itself if a handoff would take more work.

### Choose the executor model

Relay defaults to **Hydrogen High**. It can handle work that touches several files when Codex can explain the approach and verify the result. Try asking Codex to hand Hydrogen a CSV import change that needs updates to both parsing and validation, with a check that older files still import correctly.

In **Исполнитель** (Executor), choose **Flash High** or set a fixed effort for Hydrogen from the available models. Starting with 0.5.22, **Авто** (Auto) lets Codex choose **Low** for straightforward mechanical work, **Medium** for routine tasks with exceptions, or **High** for harder tasks. High is used when a task leaves the effort unspecified. **XHigh** is a manual choice.

Click **Сохранить выбор** (Save selection) to apply your choice to new tasks. The threshold decides when Codex considers handing off a task. The effort decides how deeply Hydrogen works on it.

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
