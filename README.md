# SarcoinSwap Token Lists

## Install
use [Bun](https://bun.sh/)
```sh
curl -fsSL https://bun.sh/install | bash
```

```sh
bun install
```

This repo contains main SarcoinSwap token list and tools to validate it.

## How to add external lists

URLs to external lists are stored in `token-lists.json`, if you want your list to be available on upcoming list UI page - submit a PR modifying `token-lists.json`.

## How to add new lists within this repository

- Add an array of tokens under `src/tokens`
- Add `checksum newlistname`, `generate newlistname`, `makelist newlistname` command to `package.json` analogous to SarcoinSwap default and extended list scripts.
- Modify `checksum.ts`, `buildList.ts`, `ci-check.ts`, and `default.test.ts` to handle new list

## How to add new tokens to SarcoinSwap (extended) token list

Note - this is not something we expect pull requests for.
Unless you've been specifically asked by someone from SCS team please do no submit PRs to be listed on default SCS list. You can still trade your tokens on SCS exchange by pasting your address into the token field.

- Add new tokens to `src/tokens/sarcoinswap-extended.json` file
- Run `bun makelist sarcoinswap-extended`
  - By default new list will have patch version number bumped by 1 (e.g. `2.0.1` -> `2.0.2`).
  - If you want to bump minor version add `minor` after makelist command `bun makelist sarcoinswap-extended minor`
  - If you want to bump major version add `major` after makelist command `bun makelist sarcoinswap-extended major`
- If tests pass - new token list will be created under `lists` directory

For list to be considered valid it need to satisfy the following criteria:

- It passes [token list schema](https://github.com/Uniswap/token-lists/blob/master/src/tokenlist.schema.json) validation (schema is [slightly modified](src/schema.ts))
- There are no duplicate addresses, symbols or token names in the list
- All addresses are valid and checksummed (`bun checksum listName` automatically converts addresses to checksummed versions, it is also part of `bun makelist listName`)

## How to update Top100 Token list

Note - this is not something we expect pull requests for.

```shell script
# Fetch the Top100 Tokens on SarcoinSwap v2, and update list.
$ bun fetch sarcoinswap-top-100

# Build token list (sarcoinswap-top-100.json)
$ bun makelist sarcoinswap-top-100
```

## Deploying

Token lists will be auto-deployed via Cloudflare Pages when PR is merged to master. Be sure to build the list with `bun makelist list-name` before submitting/merging the PR since it doesn't make much sense building lists within Pages (because most errors are related to wrong token information and should be fixed prior to landing into master)

Pages simply takes the json files under `lists` directory and hosts them on `tokens.sarcoinswap.finance/list-name.json`
