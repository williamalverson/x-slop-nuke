# x-slop-nuke

Shared block lists for cleaning slop out of your X For You feed. Each list is a CSV of X accounts in one category. The Grok Bot skill "X slop nuke" reads these files and blocks them on your own X account.

## Lists

| Category | File | Count | What's in it |
|---|---|---|---|
| gambling | `lists/gambling.csv` | 1529 | Sportsbooks, online and crypto casinos, sweepstakes casinos, DFS apps, prediction markets, plus their regional and fake accounts, affiliate-code creators, tipsters, gambling streamers and promo bots |
| streamers | `lists/streamers.csv` | 450 | Streamers and their clip channels, fan accounts and aggregators on Twitch, Kick and YouTube. Off by default. |
| crypto | `lists/crypto.csv` | 908 | Degen traders, callers, memecoin shills, meme-stock pumpers, airdrop farmers, paid signal groups and giveaway bait. Includes official platform accounts by design. Off by default. |
| onlyfans | `lists/onlyfans.csv` | 717 | Creators, promo groups, agencies, repost accounts and spam bots on OnlyFans and similar platforms. Includes official platform accounts by design. Sensitive and opt-in. Off by default. |

`categories.json` lists every category and file, so new categories can be added without changing the skill. Its `version` is a date. Each category has `sensitive` and `default_on`. Gambling is not sensitive and is on by default. Streamers, crypto and OnlyFans are off by default. OnlyFans is sensitive and opt-in. The skill leaves sensitive categories off unless you opt in.

`removed_file` in `categories.json` points at `lists/removed.csv`, the accounts taken off a list. On your next run, entries you blocked through this project are unblocked only after you say yes.

## File format

Each list CSV has these columns:

- `handle`: X handle without the @
- `name`: display name when the list was built
- `category`: the list it belongs to
- `type`: official, unofficial-brand, affiliate-creator, tipster, streamer, staff, promo-bot, whale-alert, betting-media or other
- `brand`: the brand it's tied to, when there is one
- `added`: date it was added (YYYY-MM-DD), used to find new accounts since your last run
- `user_id`: X numeric user id, as a string. It may be blank. The gambling list has one on every row. Those ids were looked up from each handle through X's username lookup on 2026-11-22, so they show who held the handle that day and aren't verified against earlier owners. Match on `user_id`, and fall back to the handle, ignoring case, only when `user_id` is blank. The skill resolves that id at block time and saves it.

The removed list has a header and these columns. It has no rows yet.

- `handle`: X handle without the @
- `user_id`: X numeric user id, as a string, when known
- `category`: the list the account came from
- `reason`: why it was removed
- `removed`: date it was removed (YYYY-MM-DD)

## How to use it

Run the "X slop nuke" skill in Grok Bot with your X account connected. It downloads the lists you pick. It skips anyone you follow unless you say to block them too, and it never blocks accounts in your personal keep list (`keep.txt`). Sensitive categories stay off unless you opt in.

It asks before it blocks, then blocks in daily batches under X's limit of about 1,000 blocks a day. An optional weekly check blocks only accounts added since your last run, either automatically or after showing you first. Accounts in `lists/removed.csv` are unblocked on your next run only after you say yes.

The skill only blocks and unblocks. It never posts, follows, likes, replies, DMs, or reports.

## Notes

- Inclusion means an account promotes or looks tied to the category. It is not a claim about any person or company.
- Blocking also removes follows between you and that account. You can unblock anyone in X settings.
- Think an account doesn't belong? Open an issue.
