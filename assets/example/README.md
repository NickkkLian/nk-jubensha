# Example: 《九点半的声音》 (The Voice at Half Past Nine)

A short five-player game written with this skill and verified by the detective loop.

- **Premise.** Shanghai, the night of 10 December 1936. The owner of a small radio station is found dead in the
  record library. For the fifteen minutes in which he died, the whole building could hear the station's star announcer
  reading the 9:30 news.
- **Cast.** 温仲言 (the announcer), 管小川 (the engineer), 卢佩兰 (the owner's secretary), 苏棠 (the station's
  singer), 邵景行 (the young heir of the station's biggest advertiser). Every one of them lies about something.
- **Trick.** Family T3, presence faked (T3.b, a recorded presence). How it is done is in `booklets/host.html`, which
  players should not open.
- **Size.** Two to two and a half hours with fifteen minutes of reading; each booklet is roughly a thousand Chinese
  characters. Two search rounds (each player draws one card per round; the culprit may hold back one card), 31 clues
  (12 search cards, 4 host announcements, 15 held in booklets), 12 solution steps, 3 red herrings.

## Play it

1. Print `booklets/index.html` (the cover and the cast), one booklet per player (`WEN.html`, `GUAN.html`, `LU.html`,
   `SU.html`, `SHAO.html`), and `clues.html`; cut the clue cards apart by round.
2. The host reads `booklets/host.html` alone beforehand. It has the flow for each round, what to hand out and when,
   hints from mild to strong, the reveal and the vote.
3. Players read act 1, introduce themselves, search, talk it over, read act 2 when the host says so, search again,
   put every card on the table, argue and vote. The last vote, about who looks after the culprit's sister, comes after
   the reveal.

## Check it yourself

```bash
python3 scripts/chain_check.py assets/example/game.json          # 0 errors, 0 warnings
python3 scripts/booklets.py assets/example/game.json --out /tmp/booklets
python3 scripts/detective.py prompt assets/example/game.json --out prompt.md
```

`detective/` holds the detective runs, including the draft-1 control that exposed two unintended routes to the culprit,
and what the later drafts changed (one route closed, one weakened; its README says which and shows the chains). Game text is Chinese; the notes around it are English.
