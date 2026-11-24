---
name: t3-code
description: What T3 Code can do (threads, delegation, worktrees, linked environments, previews, devices, scheduling, PR/issue linking, CLI) and when to use each. Use when the user asks what T3 Code can do, or a task needs a T3 feature.
---

# T3 Code

T3 Code runs agent sessions (Claude, Codex, Cursor, Antigravity and others) as **threads** inside **projects**. It exposes MCP tools named `mcp__t3-code__*` and a `t3` CLI at `~/.t3/bin/t3`.

Sources: the T3 Code system-prompt sections, the MCP tool descriptions, and `t3 --help`, all read on 2026-11-24. Anything I could not confirm is marked **(unconfirmed)**.

**Ordinary coding tasks need no T3 tools.** Reach for them only when a task needs one of the features below.

T3 MCP tools are **deferred**. Load them with `ToolSearch` first, e.g. `select:mcp__t3-code__delegate_task` or `+t3-code preview`. They can load late, so try the name once before you say a tool is missing.

---

## Decision table

| I want to… | Use |
|---|---|
| Run a subagent on my own provider/model with no extra settings | Native `Agent` (Claude, `run_in_background:false`) or `spawn_agent` (Codex) |
| Run a subagent on another provider or model, with custom settings, in the background, or in its own checkout | `delegate_task` (call `orchestrator_capabilities` first) |
| See which providers and models I can delegate to | `orchestrator_capabilities` |
| Check on or stop a delegate | Wait for its completion wake. Use `task_status` only after that, and `task_cancel` to stop it |
| Start a new top-level thread (only when the user asks for one) | `t3_thread_launch` (one thread, optionally a worktree) / `create_threads` (up to 20, same checkout) |
| Message a thread, or wait for it to finish | `t3_thread_send` / `t3_thread_wait` |
| Read a thread the user named by id | `t3_thread_read` (always before answering about it) |
| Find or list threads | `t3_thread_search` / `t3_thread_list` |
| Settle, pin, snooze or archive a thread | `t3_thread_organize` |
| Fork a thread or merge context back | `t3_thread_transfer` |
| Move this thread into a new worktree | `t3_worktree_handoff` (call it last in the turn) |
| Work on another of the user's machines | `t3_environment_links`, then pass `environmentId`. Not linked yet: `t3_environment_link` |
| Show a chart, diagram or mockup inline | `html_preview`, then `html_render` |
| Drive a web page or dev server (only when asked) | `preview_open`, `preview_snapshot`, `preview_act`, … then `t3_preview_close` |
| Drive an iOS Simulator or Android Emulator | `device_list`, `device_open` (gives the `agent-device` command), `device_screenshot` |
| Show the user something in full mid-turn | `send_to_user` |
| Get a password, token or pairing code from the user | `request_secret` |
| Keep the thread "Working" while my background job runs | `t3_background_wait` |
| Schedule a recurring, one-time or webhook-triggered prompt | `schedule_task` |
| Open a PR | `pull_request_create` (pushes and links it) |
| Link a PR I created or pushed to | `link_pull_request` / `link_pull_requests` |
| Monitor or babysit a PR | `watch_pull_request`, then end the turn |
| Read, review, reply to, edit or merge a PR | `pull_request_read` / `pull_request_review` / `pull_request_respond` / `pull_request_update` / `pull_request_merge` |
| Read CI logs or re-run checks | `ci_logs` / `ci_rerun` |
| Work on or file an issue | `link_issue`, `read_issue`, `create_issue`, `comment_on_issue`, `set_issue_state` |
| Manage projects | `t3_project_*` or `t3 project add/remove/rename` |
| Script T3 from a shell | `t3 thread start/send/show/list` |

---

## Subagents and delegation

**Choose the tool**
- Use the **native subagent** (`Agent` with `run_in_background:false`, or Codex `spawn_agent` then wait) when the child uses your provider, a model it offers, and no settings beyond yours.
- Use **`delegate_task`** for any of these: another provider's model, settings the user asked for, a separate checkout (`isolation:"worktree"`), or a delegate named by the user. A native agent type that runs on another provider's model also counts.
- A **background** agent is always `delegate_task`, never a background `Agent`.
- Use a Workflow only when the user asks for one.

**`delegate_task`**: starts one child that sees only `task`, with no parent history. Never put secrets in `task`.
- `target`: `{providerInstanceId, model, options, environmentId, projectId}`. Settings go in `target.options`, and option ids differ per provider.
- `isolation`: `shared` (the default, the parent checkout) or `worktree` (a new checkout from the parent commit, so uncommitted edits are not copied). Use `worktree` when each child needs its own checkout or PR.
- `mode`: `async` (the default) or `wait`. Use `wait` only if this turn needs the result. Its timeout is at most 45 s and does not cancel the child.
- `role`: implementation, research, review, design, test or general.
- `runtimeMode`: set it only if the user asks for a supervised child. A mode broader than yours is rejected.

**`orchestrator_capabilities`**: lists the providers, models, options and runtime modes. Call it once per thread before the first `delegate_task`. Call it again only after an error or when the user names a different provider. Use `search` to find models the short list leaves out.

**Rules**
- If a provider or model is unavailable, say so. Never switch to another one unasked, and never fall back to native tools, Workflow or CLIs.
- Never create a child's checkout yourself, in the shell or in `task`.
- **Never poll a delegate.** That means no `task_status` on a running task, no `sleep`, no watchers, no Monitor and no ScheduleWakeup. Follow its `next` and let its completion wake you.
- `task_status` returns `workState` (working, waiting_for_children or result_available) and `summary`. Reading it marks the result delivered.
- `task_cancel` stops a delegate along with its turns, its own delegates and its PR watches. Use it to stop a delegate, never `t3_thread_interrupt` on a `childThreadId`. Also cancel the extra delegates when the user asks for fewer, and start no replacements.
- Each new delegate review or re-check is a new `delegate_task` carrying the brief, findings and objections. Never send it with `t3_thread_send`.
- Fix small review findings yourself instead of starting a delegate for them.
- As a child, do the work yourself, and start no subagents unless your task says to.

**Snapshot from 2026-11-24, machine-specific.** Always call `orchestrator_capabilities` for current data.

| Provider | Can delegate | Models it listed |
|---|---|---|
| `claudeAgent` | yes | claude-opus-5-5, claude-sonnet-5-5, claude-fable-5-2, claude-haiku-5-5. Options: `effort` (low…ultrathink), `ultracode`, `fastMode` |
| `codex` | yes | gpt-6.2-sol, gpt-6.2-astra, gpt-6.1-terra, gpt-6.1-sol, gpt-6-astra, gpt-6-luna. Options: `reasoningEffort`, `serviceTier` |
| `cursor` | yes | auto-smart, grok-4.7, grok-4.6, composer-2.5, claude-opus-5-5, claude-opus-5, claude-opus-4-8, and 42 more |
| `antigravity` | yes | gemini-3.8-flash, claude-sonnet-4-6, claude-opus-4-6-thinking, gpt-oss-120b-medium. Option: `thought_level` |
| grok, opencode, fx, kiro, muse, pi | no | The instance is disabled |

---

## Threads

**When to create one:** `t3_thread_launch` and `create_threads` make new **top-level** threads. Use them only when the user asks for a new thread. "Subagent", "delegate" and "parallel" never mean a new thread.

**The tools**
- `t3_thread_launch`: creates one thread with `title` and `message`. By default it inherits your project, model and modes, and its modes cannot exceed yours.
  - Pass `workspaceStrategy` for a worktree thread. See [Worktrees](#worktrees) for which base to choose.
  - Without `workspaceStrategy`, the thread runs in the project root, not in your worktree.
  - Other options: `scratch:true` (a new Scratch folder with no project) and `outputSchema` (the thread must return structured data).
  - After an error or a lost reply, retry with the same `clientRequestId`. Follow up with `t3_thread_read` or `t3_thread_wait`.
- `create_threads`: creates up to 20 threads in **this** checkout, inheriting project, branch and worktree.
- `t3_thread_send`: sends a message to any thread, using `mode` `auto`, `queue`, `steer` or `restart`.
  - `freshContext` starts a new session, like `/clear`. `outputSchema` asks for a structured result.
  - Sending to a finished child thread starts a fresh task and returns its `taskId`.
- `t3_thread_wait`: waits for a run to finish. Options: `untilDelegatedWorkSettled`, `untilOutput`. If it times out and returns `next`, end your turn. If it times out without `next`, call it again.
- `t3_thread_read`: reads a thread, with `view` set to `messages` or `activity`, newest first. Paginate with `beforePosition`/`afterPosition`, and continue cut-off text with `itemId` plus `textOffset`.
- `t3_thread_list`, `t3_thread_search`: list threads, or search them by title, branch, PR, project and then message text. Search skips subagent threads and is not exhaustive.
- `t3_thread_configure`: reads or sets a thread's provider, model and options. Permission modes stay as they are.
- `t3_thread_update`: renames a thread or regenerates its title.
- `t3_thread_organize`: pin, unpin, snooze (needs `snoozedUntil`), unsnooze, settle, unsettle, archive, unarchive or mark_unread. When asked to settle threads, include this one too. Settling this thread takes effect at the end of the turn.
- `t3_thread_transfer`: `fork` copies a thread into a new one, `merge_back` merges it into a related thread, and `status` reports transfers.
- `t3_thread_interrupt`: interrupts the running turn of a **top-level** thread. For a delegate, use `task_cancel` instead.
- `t3_queue`: lists, reads, edits, cancels or reorders queued messages, or sends one now as a steer (`promote_to_steer`).
- `t3_pending_request`: lists, reads or answers a thread's pending user questions. It cannot approve permission requests.
- `t3_thread_send_attachments`, `t3_attachment_prepare_upload`, `t3_attachment_discard`: upload a file (POST its bytes to `uploadUrl`) and send it to a thread.
- `t3_submit_output`: returns this turn's result as JSON when the message asked for an output schema.
- `t3_workflow_stop`: stops a running Claude workflow in a thread. The thread itself keeps running.

**Rules**
- When you mention a thread in a reply, use its `link` exactly as returned, never its title or bare id.
- A thread you message cannot run with broader modes than yours.

---

## Worktrees

- To give a **separate agent** a worktree, use `t3_thread_launch` with `workspaceStrategy`, or `delegate_task` with `isolation:"worktree"`.
- To move **this thread** into a new worktree, use `t3_worktree_handoff` (`branch`, optional `baseRef`, `continuationPrompt`, `startFromOrigin`).
  - Check `t3_worktree_status` first. The handoff fails if this thread already has a worktree.
  - Call it last in your turn, because the move ends the live session. With `continuationPrompt`, work resumes there next turn. Without it, the thread waits.
- `t3_worktree_list`: lists the branch refs and checkout paths in a thread's workspace.

**Choosing a base (`workspaceStrategy`)**
- If the user asked for a base, use it.
- Independent work: start from the default branch as `remote/branch` (e.g. `origin/main`) with `startFromOrigin:true`. With no remote, use the local default branch.
- Work that needs unpublished commits, or dependent and stacked work: use the local base with `startFromOrigin:false`.
- Types: `worktree` (a new checkout), `existing_worktree` (bind `worktreePath`) and `root` (the project checkout, the default).

**Rules**
- Uncommitted edits are never copied into a new worktree.
- A worktree you create from the shell is not bound to the thread. Never put checkout setup in a thread's prompt.

---

## Linked environments (other machines)

**Tools**
- `t3_environment_links`: lists the linked machines and whether each one answers now. To act on one, pass its `environmentId` to a T3 tool. `delegate_task` takes it as `target.environmentId`.
- `t3_environment_link`: asks the user, in a card, to link a machine they named. Pass `hint` from their words (e.g. "vps"), and `url` only if the user gave an address.
  - It returns `pending`, and the outcome arrives later as a message in the thread. Do not wait, poll or call it again.
  - **Never ask for a pairing code in chat.**
  - If the link tools are missing, tell the user to enable Settings → Labs → Linked environments.
- `t3_environment_unlink`: forgets a linked machine.
- `t3_environment_read`: reads this server's identity and preferences.
- `t3_environment_preferences_update`: changes environment-wide preferences: default thread env mode, `newWorktreesStartFromOrigin`, source-control writing style, background activity profile and provider update checks.

**CLI**
- `t3 environment link [<url>] --pairing-code <code> --access <read-only|approval-required|auto-accept-edits|auto|full-access>`. `--from <file>` links several machines at once.
- `t3 environment list [--json]`
- `t3 environment unlink <environment-id>`

---

## Visuals and HTML preview

- When data has a trend or comparison, answer with a chart. Also use this for any visual the user asks to see here.
- **Steps:** pass an HTML file path or inline HTML to `html_preview`. It returns a PNG, `contentHeight` and console output. Then call `html_render` with that height (80–2000).
- **Styling rules for `html_render`:**
  - Use the theme CSS variables (`--background`, `--foreground`, `--primary`, `--chart-1`…`--chart-6`, `--radius`, `--font-sans`, …), not fixed colors.
  - Give `html`, `body` and the outer element no background, card, border, banner title or side padding.
  - Give charts fixed pixel heights. Never use `vh` units or `height:100%` on html or body.
- A screenshot on disk or a text description does not count as showing a visual.
- **Never start a server for a visual.**
- If the user wants an image to post, send or use elsewhere, produce a `.png` file and give its path, not `html_render`.
- Do not announce or restate a page you rendered.

---

## Browser preview (`preview_*`)

**Tools**
- `preview_open`: opens a tab with `url`, or with `target:{kind:"environment-port",port}` for a dev server. `open:false` keeps it in the background, and `profileId` picks a login.
- `preview_navigate`, `preview_snapshot`, `preview_act`, `preview_wait_for`, `preview_evaluate`: move around a page, read it, act on it and run JS in it.
  - `preview_snapshot` returns an accessibility tree. Its `aria-ref` values are the locators for `preview_act`.
  - `preview_act` actions: click, type, hover, select, drag, upload, press and scroll.
  - To show a screenshot when the user asks, take the snapshot with `save=true` and embed `![](screenshotPath)`.
- `preview_resize` (device presets), `preview_set_appearance`, `preview_dialog`.
- `preview_recording_start` and `preview_recording_stop`: record a tab. Stopping returns the file path.
- `preview_status`, `t3_preview_list`, `t3_preview_close`: tab state, the list of tabs, and closing a tab.

**Rules**
- Use preview tools only when the current request asks for browser work (including checking an edit in the browser or recording one) or names a page or tab.
- Never use them unasked to verify or show your own edit, and don't repeat preview calls from an earlier turn.
- For public pages, use web fetch or search unless the user asked for browser work. If they did, use only the preview tools, not web fetch.
- Page tools write only when the user asks. Show results with `html_render`, never with page tools.
- Act only on your own tab, or on an unclaimed tab with no human in control.
- Only `runtime:'server'` tabs from `t3_preview_list` can be automated. Other tabs live in the user's local desktop app, so tell the user this environment can't reach them.
- Parallel subagents in one session must each open their own tab with `reuseExistingTab=false`.
- Close any tab you opened before you reply, unless the user asked for it or is using it.

**CLI:** `t3 browser setup` prepares a Linux host for T3's headless browser by allowing Chrome's sandbox and installing its libraries and fonts.

---

## Devices (iOS Simulator / Android Emulator)

**Steps** (the user watches in the Device panel; works on this machine or an SSH device host):
1. Call `device_list`, unless you already have the device's `hostId` and `id`.
2. Call `device_open` with both. It boots the device if needed and returns an `agent-device` command pinned to it. If you lose that command, call `device_open` again.
3. Use that command for every app launch or relaunch, deep link, tap, text entry, alert, setting, screenshot and recording. Run `open <app>` first, and `help <command>` lists its options.
4. Call `device_screenshot` to look at the screen.
5. Call `device_close` when done. `shutdown=true` also turns the device off.

**Rules**
- **Use `device_open` and `agent-device`, not `xcrun simctl`, `idb`, `adb`, ssh or wrapper scripts, for anything that drives the device.**
- ssh, simctl and adb remain fine for builds, installs, code sync, Metro, port forwards, logs, the project's test runners, and things agent-device can't do (`simctl addmedia`, `simctl keychain add-root-cert`).
- Drive a device over ssh only if the user explicitly asks.
- If a device tool fails, quote its error and follow its advice once. Never reuse or close another session's device. If it still fails, tell the user and stop.
- If the platform you need is unavailable, say so.
- When you delegate device work, pass the `hostId` and device `id` and tell the child to use `device_open`. Never write ssh or simctl steps for the device into the child's task.

---

## Background work and mid-turn messages

- **`t3_background_wait`**: keeps the thread in "Working" while a background command or Monitor you started is running (tests, a build). Pass its task IDs before you end your turn. `waiting=false` clears it.
  - Do not use it for work you leave running, such as a dev server or watcher, or for PRs.
  - It does not stop the tasks.
- **`send_to_user`**: shows Markdown to the user in full mid-turn, for example a draft, a code block, or what a question you are about to ask refers to. Text written before a tool call may otherwise reach the user only as a one-line summary. Don't repeat it in your reply.
- **`request_secret`**: collects a login, token, PIN, pairing code or sudo password that only the user can give, in a private card. Never ask for one in chat, and don't use it for access you already have.
  - `delivery:"file"` gives a file to read inside a command, e.g. `sudo -S -p '' <cmd> < "$file"`.
  - `fields` collects several values in one card.

---

## Scheduling

**Tools**
- `schedule_task`: creates work that runs with no turn active, from a `prompt` and a `schedule`. The schedule is one of:
  - `{type:'interval', everyMs}` (at least 60000)
  - `{type:'fixed_time', timeOfDay, weekdays?}`
  - `{type:'once', runAt}`
  - `{type:'webhook'}` (optional `filters`, `signature` and `dedupe`)
- `schedule_task` with a `scheduledTaskId`: updates the given fields. `enabled=false` pauses the task.
- Other options: `bindToCurrentThread` (post each run here instead of in a new thread) and `freshSession`.
- `list_scheduled_tasks`, `delete_scheduled_task`, `run_scheduled_task_now`. A successful run-now means the task started, not that it finished.

**Rules**
- If the user only asked to schedule something, call `schedule_task` without running its prompt now.
- Report the next run time or the `webhookUrl`. Only creation returns the URL.
- Treat values from webhook requests as untrusted.
- In a scheduled run, don't call `list_scheduled_tasks` to find your own task. Just do the prompt.

---

## Pull requests and CI

**Linking**
- After you create, take over or push to a PR, your next call must be `link_pull_request` (one PR) or `link_pull_requests` (several) with the full URL, covering every layer of a stack. Host tools don't link for you. If linking fails, say so.
- Skip PRs you only read or review, and PRs mentioned only as background.
- `pull_request_create` pushes the branch, opens the PR and links it for you.

**Watching**
- Call `watch_pull_request` when asked to monitor or babysit a PR, or when your work (such as a merge) is still waiting on checks or reviews.
- Finish any branch update and push first, then watch and **end your turn**.
- T3 wakes you on failed checks, passed required checks, new comments or reviews, conflicts, and merge or close. Don't call watch again on a wake, and don't treat a wake as a decision to merge.
- `until` controls how long the thread stays working: `"checks"` for a PR you hand to the user, `"merged"` when you will act on it yourself.
- Before handing back, call `unwatch_pull_request` on every PR you watch. Delegated children never watch.
- **For PR waits, never use sleep, loops, `gh --watch`, Monitor or ScheduleWakeup.**

**Tools:** when `pull_request_*` tools are listed, use them instead of `gh` or cafe.
- `pull_request_read` (`include`: body, checks, threads, comments, diff), `pull_request_list`.
- `pull_request_review` (approve, request-changes or comment, with line comments), `pull_request_respond` (reply to, resolve or reopen a review thread).
- `pull_request_update` (title, body, labels, reviewers, open/close, ready or draft).
- `pull_request_merge` needs `expectedHeadSha` from `pull_request_read`. Options: `method`, `auto`, `stack`.
- `ci_logs`, `ci_rerun`: read failing check logs, or re-run checks.
- `write_pull_request_guides`, `read_pull_request_guide`: create or read the PR Guide tab tour.
- `list_thread_pull_requests`, `unlink_pull_request`, `unlink_pull_requests`.

**After PR work, a child's last line is:** `PR <full URL>, head <SHA>, checks and reviews <state>.`

---

## Issues

- Call `link_issue` for each issue you take on or file, even one filed via a CLI. Skip issues you only read. Linked issues show beside the thread.
- `list_issues`, `read_issue`: list a project's issues, or read one. `read_issue` also lists the states an issue can move to.
- `create_issue`, `comment_on_issue`, `set_issue_state`.
- `list_thread_issues`, `unlink_issue`.
- Which trackers are supported is **(unconfirmed)**. The tools say only "a project's tracker".

---

## Projects and theme

- `t3_project_list`, `t3_project_read`, `t3_project_create`, `t3_project_update`, `t3_project_delete`. Delete keeps the folder on disk, and a nonempty project needs `force:true`.
  - `t3_project_create` without a `workspaceRoot` makes a new git repo with a README and a first commit.
  - Project settings include scripts (run on worktree create or on settle, auto-open preview), tracked folders, the default model, the default thread env mode and the icon.
- `t3_project_clone`: clones a repository. Register it afterwards with `t3_project_create`, using the same `environmentId`.
- `t3_theme_read`, `t3_theme_set`: restyle T3 Code itself, **only when the user asks**. Read the guide first.

---

## The `t3` CLI (`~/.t3/bin/t3`)

| Command | What it does |
|---|---|
| `t3 [<cwd>]` | Runs T3 Code in the terminal and opens it in the browser. Flags: `--port`, `--host`, `--lan`, `--tailscale-serve`, `--no-browser`, `--mode web\|desktop` |
| `t3 serve` | Runs T3 Code with no browser |
| `t3 app [<path>]` | Opens a project in the running desktop app |
| `t3 thread start <project> <message>` | Starts a thread and prints its id. Flags: `--title`, `--generate-title`, `--provider` plus `--model`, `--option id=value`, `--worktree <ref>`, `--branch`, `--output-schema <file>`, `--wait`, `--json`. `--wait` exits 1 if the run fails and 2 if it waits for an answer. Pass `-` as the message to read it from stdin |
| `t3 thread send <thread> <message>` | Sends a message. A busy thread gets it as its next turn, and nothing is interrupted. Flags: `--output-schema`, `--wait`, `--json` |
| `t3 thread show <thread>` | Shows a thread's state and latest messages (`--messages N`, `--json`) |
| `t3 thread list` | Lists a project's threads (`--project`, `--json`) |
| `t3 project add <path>` | Adds a project (`--title`, `--track <folder>`, repeatable) |
| `t3 project remove <project>` | Removes a project. `--force` also deletes all its threads |
| `t3 project rename <project> <title>` | Renames a project |
| `t3 environment link\|list\|unlink` | See [Linked environments](#linked-environments-other-machines) |
| `t3 pair` | Mints a pairing token for a running server and prints it as a QR code (`--scope` repeatable, `--ttl` with a 5 min default, `--label`, `--tailscale`) |
| `t3 auth pairing\|session\|connect-device` | Manages one-time pairing tokens, bearer sessions and T3 Connect devices. The subcommands under these are **(unconfirmed)**; I didn't run their help |
| `t3 browser setup` | Prepares a Linux host for T3's headless browser |
| `t3 workflow start\|list\|show\|resume\|cancel\|signal\|questions\|hooks\|store` | Durable workflow runs |
| `t3 up` | Sets up remote access and the background service (`--connect`, `--lan`, `--yes`, `--headless`). A restart stops running agent turns |
| `t3 connect` | Same as `t3 up --connect` |
| `t3 status`, `service`, `update`, `install`, `uninstall` | Service setup and the t3 install itself |
| `t3 theme`, `trace`, `doctor`, `policy` | Theme defaults, the server trace file, repairing the data directory, the managed policy |
| `t3 triage`, `feedback` | Investigate a T3 problem and file an issue; send feedback to the T3 team |

All CLI commands accept `--base-dir`, which sets the data directory (same as `T3CODE_HOME`).
