# nk-jubensha

![nk-jubensha](https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/social/nk-jubensha.png)

A [Claude Code](https://code.claude.com) skill. Build a playable jubensha (剧本杀, the Chinese murder-mystery party game) from a premise - a cast, a trick chosen from an abstract taxonomy, a clue chain in which every deduction stands on a clue some player actually receives, and printable HTML booklets (one per player, clue cards, a host handbook).

Part of [nickkk-skills](https://github.com/NickkkLian/nickkk-skills) — agent skills that ship a self-test with every script; the Verify
section below says which of them were broken on purpose before release to prove they react.

![nk-jubensha demo: one premise in, a game a blind detective solved](https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/nk-jubensha.gif)

## What it does

- Nine stages into one `game.json`: premise and scale, a cast whose goals are find / hide / bond, a trick chosen from an abstract eight-family taxonomy, the true timeline, a clue chain where every deduction cites the clues it stands on, and booklets split into acts and into what each character tells the table and what they keep.
- `chain_check.py` finds steps that need a clue nobody receives, orphan clues, red herrings that never resolve, a cover that breaks through the wrong clue, the answer leaked into a booklet, culprits found by elimination alone, and roles with nothing to do.
- `booklets.py` prints one HTML booklet per player, the clue cards and a host handbook; single files, offline, A4.
- `detective.py` builds what the table knows (and refuses if it would leak the answer), runs a blind second session in Claude Code or Codex, and grades it: SOLVED in N steps, RIGHT NAME / COVER NOT BROKEN, WRONG, or UNSOLVABLE with the missing clue named. A grade is marked UNVERIFIED unless the answer comes with the session's own record, word for word: in testing, one agent condensed its detectives' answers before grading them, and another, whose command-line session failed to start, wrote the answers into files itself.
- The example game, 《九点半的声音》 (five players, a 1936 radio station), ships with its detective transcripts, including the draft-1 control run that exposed an unintended route to the culprit and what changed after it (the weapon route closed; the note route weakened, not shown closed; see `assets/example/detective/README.md`).
- Standard library only. Every chain_check.py rule has a sample only it catches; every line of it that reports a finding, and the guarded lines of detective.py, were broken on purpose to prove the self-tests go red.

The full procedure, the boundaries and where the rules came from are in [SKILL.md](SKILL.md).

## How it works

1. Premise and scale
2. Cast
3. Trick
4. Truth
5. Clue chain
6. Booklets
7. Check
8. Render
9. Play it blind

## Why it is built this way

**The idea.** A premise becomes a game a group can sit down and play: five or six characters, each with a booklet that is fun to read alone and incomplete on purpose; a trick; clue cards for two search rounds; a host handbook. A script checks the structure, a renderer prints the booklets, and then the part a writer cannot do for themselves: a second model that never saw the solution plays detective on exactly what the players get, and says whether it solved the case, and if not, which clue it was missing.

**Where it came from.** The blind detective comes from the author's practice of having someone who did not do the work judge the evidence (see nk-evidence-audit): a writer who knows the answer cannot test whether the clues are enough. The rules about the detective's answer, and the grade's run-record check, came from testing this skill in clean sessions: one agent condensed its detectives' answers before grading them, and another, whose command-line session failed to start, wrote the answers into files itself and reported a blind result. The trick taxonomy is abstract and written for this skill.

**Evidence.** What was broken on purpose to show that the self-tests can fail is under [Verify](#verify); what was run end to end, and in which agent, is under [Compatibility](#compatibility).

## Install

Pick one of four ways: three for Claude Code, one for OpenAI Codex. Skills load when a session starts, so open a **new** session after installing.

### 1 · Terminal, one command

```bash
git clone https://github.com/NickkkLian/nk-jubensha ~/.claude/skills/nk-jubensha
```

1. Run the command above (for one project only, clone into `.claude/skills/nk-jubensha` inside that project).
2. Start a new Claude Code session.
3. Check it loaded: type `/nk-jubensha` — it appears in the slash-command menu. Or just ask for the task; the skill triggers on its own.

### 2 · Claude Code in a terminal session (plugin)

The plugin route goes through the [nickkk-skills](https://github.com/NickkkLian/nickkk-skills) marketplace. Add it once; after that each skill is one command.

```
/plugin marketplace add NickkkLian/nickkk-skills
/plugin install nk-jubensha@nickkk-skills
```

1. In a Claude Code session, run the first line (once per machine).
2. Run the second line.
3. Start a new session (or run `/reload-plugins`). The skill shows up as `nk-jubensha:nk-jubensha`.

Without opening a session, the same two steps work from a shell: `claude plugin marketplace add NickkkLian/nickkk-skills` then `claude plugin install nk-jubensha@nickkk-skills`.

### 3 · Claude desktop app (Code tab)

**Add the marketplace first — Discover only searches marketplaces you have already added.**

<img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/panel-route.gif" alt="Adding the marketplace and installing a skill in the desktop app" width="640">

<sub>Recorded on 2026-09-16, when the marketplace listed ten skills, all at version 0.1.0; it lists more now. The repository list in this recording shows the recorder's own repositories because a GitHub account is connected; yours will show yours. Type the full name as in step 4.</sub>

1. In the chat box, type `/plugin marketplace` and press Enter (or open **Settings → Customize → Plugins**). The **Plugins** panel opens.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step1-type-plugin-marketplace.png" alt="/plugin marketplace typed in the chat box" width="480">
2. Top right, open **Add ▾** and choose **Add marketplace**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step2-add-menu.png" alt="The Add menu with Add marketplace" width="480">
3. Choose **Add from a repository**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step3-add-from-repository.png" alt="Add marketplace dialog: Add from a repository" width="480">
4. In **URL**, type the full `NickkkLian/nickkk-skills`. At the bottom of the list choose the row **Use "NickkkLian/nickkk-skills"**, then press **Sync**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step4-url-then-sync.png" alt="URL filled in, Sync button" width="480">
5. You land on **Discover**, filtered to the new marketplace (**Filter · 1**). Find **Nk jubensha** and press **Add**. Installed ones show **✓ Added**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step5-discover-add.png" alt="Discover list with Added and Add buttons" width="480">
6. Close the panel and start a new session.

To try it for one session without installing anything: `claude --plugin-dir ./nk-jubensha` from a clone.

### 4 · OpenAI Codex CLI

```bash
git clone https://github.com/NickkkLian/nk-jubensha.git ~/.agents/skills/nk-jubensha
```

1. Run the command above (for one project only, clone into `.agents/skills/nk-jubensha` inside that project).
2. Start a new Codex session.
3. Check it loaded, without spending a model call: `codex debug prompt-input | grep -o -- '- nk-jubensha[a-z0-9:-]*' | sort -u` prints `- nk-jubensha:nk-jubensha:`. Codex adds the `nk-jubensha:` prefix because this repository also carries a Claude Code plugin manifest. Ask for the task and the skill triggers on its own, or type `$` and pick it from the list.

## Compatibility

| Agent | Tested | What was checked |
|---|---|---|
| Claude Code (CLI 2.1.173, macOS) | yes | In a fresh project with an isolated Claude config, inside a macOS sandbox that blocked reading the tester's ~/.claude folder (settings, session history, memory), Desktop, Documents and Downloads, SSH keys and git identity, a plain request that never names the skill triggered it and it ran its bundled script. Here the request, in Chinese, was a five-player 剧本杀 set in a 1930s Shanghai teahouse that had to be solvable. Two runs. With the first version of this skill the session wrote a game that passed chain_check (0 errors, 0 warnings) and ran its detectives as subagents given only the prompt as input, with shell access (1 and 2 commands, not recorded), but saved shortened versions of their answers and graded those, and it replied in Japanese; the grade's run-record check and the rules about the detective's answer were added because of that run. With this version the session replied mostly in Chinese (it opened with two English paragraphs), wrote a game that passes chain_check (0 errors, 0 warnings), ran the detective with `scripts/detective.py run --engine claude` (a fresh session with no tools), and the grade, re-run here, reads SOLVED in 7 steps with a verified run record. |
| OpenAI Codex CLI (0.159.0, gpt-6.1-sol, medium reasoning, macOS) | partly | Copied into `~/.agents/skills` of a temporary home (the folder route 4 clones into), in a fresh project, without the user's Codex config, with `codex` on the PATH and network access on. From a plain English request, Codex read SKILL.md, wrote a five-player game in English that passes chain_check (0 errors, 0 warnings), rendered the booklets and built the detective prompt; in the second run the prompt builder first refused a prompt that repeated the solution summary, and Codex reworded the game until it passed. Neither recorded detective route could start in this test: `claude -p` was not signed in under the temporary home, and a nested `codex exec` stopped inside Codex's sandbox ("failed to initialize in-process app-server client: Operation not permitted"). In both runs Codex then wrote detective answers into files itself and described them as coming from fresh sub-agents; its event stream records no sub-agent being started. The grade marks those answers UNVERIFIED (no run record), and in the second run Codex reported the result as unverified. From inside Codex, a blind detective run needs `scripts/detective.py run` started from a terminal. |
| Cursor, Gemini CLI | no | Not tested. Their documentation says both read `~/.agents/skills`, the folder route 4 clones into; Gemini CLI asks before it activates a skill. |

In this skill's Codex run, every call into the skill folder's scripts/ used that folder's absolute path. This skill's frontmatter uses only name, description, license and metadata.

## Verify

```bash
python3 scripts/booklets.py --selftest
python3 scripts/chain_check.py --selftest
python3 scripts/detective.py --selftest
```

Python 3.9+, standard library only. Each of the 41 lines of chain_check.py that report a finding was read from
the source, not listed by hand, and disabled on its own in a sandbox copy; eight rules were also made to over-fire.
All 49 breaks turned the sample written for them red, none by a crash, and the unmutated control stayed green.
28 guarded lines of detective.py (what the detective's packet keeps out, what the leak guard refuses, how the
grade is decided, how the second session is started and read back) were broken the same way, each turning its own
sample red. booklets.py has a self-test but no break matrix.

## Limits

- The checks read structure, not prose. A clue can be cited by the right step and still fail to say what the step needs; that is what the blind detective is for, and a clean check is not a good game.
- The detective is a model, not a table of people. It reads every page at once and never forgets; people talk over each other, hide things and get tired. SOLVED means the information is sufficient, not that a group will find it in three hours. Play it with people before calling it done.
- The grade matches clue ids, not reasoning. A detective can cite the right clue for the wrong reason; read its chain.
- Blindness is by construction and on record, not enforced: the Claude route runs with no tools, the Codex route runs read-only and its transcript lists every command. A subagent could still read files if told where they are, so do not tell it.
- `claude -p` loads your own user-level settings and CLAUDE.md. They know nothing about the game, but if they change how the detective answers, point `CLAUDE_CONFIG_DIR` at a clean, signed-in config, as the example's runs did (`--bare` would skip them too, but accepts only an API key).
- A nested `codex exec` does not start inside Codex's own sandbox: in testing it stopped with "failed to initialize in-process app-server client: Operation not permitted" even with network access on. From inside Codex, run the `run` command in a terminal, or approve running it outside the sandbox; a `claude -p` run needs a signed-in Claude Code. Without either, the only route left is a subagent, and its grade stays UNVERIFIED.
- The taxonomy is a map for choosing, not a source of finished tricks. It names mechanisms, not works.

## License

MIT. Read a script before letting it run in your environment.
