# The INAZIP process

## Roles

- **Author** — anyone. You write the proposal and answer questions about it.
- **Editor** — a maintainer who checks the proposal is complete, assigns a number,
  and updates status. Editors judge whether it is *ready to discuss*, not whether
  they personally like the idea.
- **Implementers** — whoever writes the node code, usually after Accepted.
- **Validators** — decide in the end, by choosing to run the release or not.

## Steps in detail

### 1. Draft

Copy `template.md` into `proposals/inazip-draft-<short-title>.md` and open a pull
request. Editors will tell you within about a week whether it needs more detail.

Common reasons a draft gets sent back:

- The problem is not described, only the solution.
- The specification is prose, not rules ("make fees fairer" is not implementable).
- No thought given to what breaks for existing nodes, wallets, or explorers.
- It duplicates an existing proposal.

### 2. Review

Once assigned a number, the proposal moves to Review. This is where hard questions
happen: attack surface, economic side effects, upgrade risk, whether it can be done
without a consensus break. Update the file in place as the design changes; the
pull-request history is the record of the discussion.

### 3. Accepted

The rules are settled. Implementation starts in `inazuma-core`, behind an activation
height. The proposal file gets a `Requires` / `Implementation` link.

### 4. Scheduled

A release is tagged with the activation block. The block must be far enough ahead
that operators have real time to upgrade — at least 50,000 blocks, which is roughly
six hours at 400 ms, and normally much more for anything risky.

### 5. Live

The activation block passes. The proposal status becomes Live and the behaviour is
documented in `inazuma-docs`. A change is not finished until the docs describe it.

## Rules that are not negotiable

1. **Activation heights.** No consensus change is applied retroactively. Every
   already-produced block must still validate under the old rules.
2. **No surprise economics.** Supply, rewards, fee burn and slashing parameters are
   written down before they ship.
3. **Reversibility where possible.** Say in the proposal how the change could be
   turned off or superseded if it goes wrong.
4. **Security review before Accepted.** If the change touches signatures, consensus
   voting, or fund movement, it needs an explicit security section and a second
   reviewer.

## Rejecting a proposal

A rejected proposal stays in the repository with the reason written in its Rationale
section. Rejections are useful history — they stop the same idea being re-litigated
from scratch every six months.

## Emergencies

An active exploit does not wait for this process. Maintainers may ship a patch
release immediately, then publish an advisory within 72 hours and a retroactive
INAZIP documenting the rule change.
