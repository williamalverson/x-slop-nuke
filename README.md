# x-slop-nuke

Shared block lists for cleaning slop out of your X For You feed. Each list is a CSV of X accounts in one category. A Grok Bot skill reads these files, shows you what it will block, and blocks only after you say yes.

## Lists

| Category | File | What's in it |
|---|---|---|
| gambling | `lists/gambling.csv` | Sportsbooks, online and crypto casinos, sweepstakes casinos, DFS apps, prediction markets, plus their regional and fake accounts, affiliate-code creators, tipsters, gambling streamers and promo bots |

`categories.json` lists every category and file, so new categories can be added without changing the skill. Its `version` is a date. Each category has `sensitive` and `default_on`. Gambling is not sensitive and is on by default. Sensitive categories such as porn and OnlyFans must set `default_on` to false and require an explicit opt-in before any account in them is blocked.

`removed_file` in `categories.json` points at `lists/removed.csv`, the accounts taken off a list. Clients should unblock these for users who blocked them via this project, after confirming the count with the user.

## File format

Each list CSV has these columns:

- `handle`: X handle without the @
- `name`: display name when the list was built
- `category`: the list it belongs to
- `type`: official, unofficial-brand, affiliate-creator, tipster, streamer, staff, promo-bot, whale-alert, betting-media or other
- `brand`: the brand it's tied to, when there is one
- `added`: date it was added (YYYY-MM-DD), used to find new accounts since your last run
- `user_id`: X numeric user id, as a string. Empty until it is filled in. Consumers should match on `user_id` when present and fall back to `handle`.

The removed list has a header and these columns. It has no rows yet.

- `handle`: X handle without the @
- `user_id`: X numeric user id, as a string, when known
- `category`: the list the account came from
- `reason`: why it was removed
- `removed`: date it was removed (YYYY-MM-DD)

## How to use it

Run the "X slop nuke" skill in Grok Bot with your X account connected. It downloads the lists you pick, skips accounts you follow unless you say otherwise, asks before blocking, and blocks in daily batches under X's limit of about 1,000 blocks a day. Run it again any time, or weekly, to block only accounts added since your last run. Sensitive categories stay off until you opt in.

## Notes

- Inclusion means an account promotes or looks tied to the category. It is not a claim about any person or company.
- Blocking also removes follows between you and that account. You can unblock anyone in X settings.
- Think an account doesn't belong? Open an issue.
