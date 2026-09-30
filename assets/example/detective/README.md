# The detective loop on 《九点半的声音》

Every run below was a fresh session that had never seen the solution: `detective.py run` started it in an empty
temporary folder with the prompt on standard input, the Claude runs with every tool switched off, the Codex runs
read-only. Each transcript records zero tool calls or commands. Each run was graded with `detective.py grade`, and
every grade's second line is its run record: the answer is the transcript's last message, word for word.

A **control** run is the same game with three clues given to nobody: C07 and C08 (two players notice the announcer's
voice sink when the lights dip at 9:36) and C11 (the 9:30 news never mentions a fire everyone else was reporting).
Those are the only clues that prove the voice was a recording. A control should not come back SOLVED; if it does, the
cover breaks some other way and the trick is not needed.

| Round | What changed | Full game, Claude | Full game, Codex | Control, Claude | Control, Codex |
|---|---|---|---|---|---|
| Draft 1 | first complete draft | SOLVED in 8 steps | SOLVED in 10 steps ¹ | RIGHT NAME, COVER NOT BROKEN ² | not run |
| Draft 2 | weapon and note reworked (below) | SOLVED in 8 steps | SOLVED in 10 steps | RIGHT NAME, COVER NOT BROKEN | UNSOLVABLE |
| Draft 3 | a minor character renamed; no clue changed | SOLVED in 8 steps | SOLVED in 9 steps | RIGHT NAME, COVER NOT BROKEN | UNSOLVABLE |
| Draft 4 | booklets deepened (background, relationships, the culprit's playbook, act-2 beats); no clue or step changed | SOLVED in 8 steps | SOLVED in 9 steps | RIGHT NAME, COVER NOT BROKEN | UNSOLVABLE |
| Final | after a playability review: search rules (one card per player per round, the culprit may hold back one), the evening-wire rule and a new clue behind the missing-fire route (C30), a new clue for the singer (C31), 16-inch discs, timeline fixes, the culprit's fallback defence | SOLVED in 7 steps | SOLVED in 9 steps | RIGHT NAME, COVER NOT BROKEN | UNSOLVABLE |

Claude runs: claude-opus-4-8 (as the transcripts report it), Claude Code CLI 2.1.173. Codex runs: gpt-6.1-sol,
Codex CLI 0.159.0.

¹ This run's answer file was not written: `detective.py` passed Codex a relative `-o` path, which Codex resolves inside
its own temporary folder. The answer was recovered from the run's event stream, and `detective.py` now passes an
absolute path (self-test D24).

² **What the draft-1 control showed, and what changed after it.** With the proof removed, the draft-1 Claude detective
still named the culprit, and its chain showed two routes that do not need the trick: the weapon was the spare microphone
found back in the broadcast booth's cabinet, and during a live broadcast only the announcer could be in the booth
(chain steps 2-3, C21 + C01); and the owner's urgent note lay unread just inside the booth door, where a live announcer
"would have seen it at a glance" (step 4, C19 + C20 + C29). See `draft1-control-claude.md`. Draft 2 changed both:

- **The weapon route was closed.** The weapon moved to the record library (a brass record weight on the audition
  turntable, which anyone in that room could reach; C21 rewritten). No Claude control chain from draft 2 on ties the
  weapon to the booth: they all place it in the record library (C21 + C02).
- **The note route was weakened, not shown closed.** The note now lies under the booth's heavy soundproof door curtain,
  where the secretary who pushed it in could not see where it fell (C19 and C20 rewritten), so an unread note can be
  explained. The Claude controls of drafts 2, 3 and 4 still used it (step 5 in each: an announcer on air would have
  read an urgent note). The final Claude control did not cite it in its chain, only when clearing 邵景行 and 卢佩兰;
  C19 and C20 are unchanged since draft 2, so that one run does not show the route is gone.

There is no Codex control for draft 1, so the Codex UNSOLVABLE results from draft 2 on have no draft-1 run to compare
with; they show what the later drafts do, not that draft 2 fixed something Codex had found.

**What the controls show from draft 2 on.** Codex said the material was not enough and named the missing fact in its
own words: whether the voice actually dropped and dragged when the voltage dipped at 9:36. That is exactly C07 and C08,
which it never saw. Claude still named the culprit in every draft, through the round-2 cards that make a recording
plausible (C16-C18: a fifteen-minute disc cut that afternoon on a false pretext, an unlabeled extra disc in the booth's
rack), the motive cards (C22, C23), and an inference that a voltage dip would have slowed a disc (C15 + C09), without
any card showing that the voice actually dragged. The grader calls this RIGHT NAME, COVER NOT BROKEN: the right name
without any of the break clues C07, C08, C11, reached by suspicion and elimination rather than proof. So the control
does not stop a strong detective from reaching the right name; what it shows is that without C07, C08 and C11 the
recording can be suspected but not proved.

## Files

- `prompt-full.md`, `prompt-control.md`: exactly what the final-round detectives saw. `prompt-full.md` is
  byte-identical to `python3 scripts/detective.py prompt assets/example/game.json`.
- `final-full-claude.md`, `final-full-codex.md`, `final-control-claude.md`, `final-control-codex.md`: run facts, the
  grade, and the detective's answer word for word.
- `draft1-control-claude.md`: the control that exposed the two unintended routes.

The raw event streams are not included (they carry machine paths and session ids); the answers are as the models wrote them.
