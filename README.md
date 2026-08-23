# rolemodel/homebrew-tap

Homebrew tap for the RoleModel video pipeline.

```sh
brew install rolemodel/tap/rm-video                    # the toolkit: eight commands
brew install --cask rolemodel/tap/rolemodel-openscreen # the app
```

Most people should not run either of those directly — one command does the whole
thing, including the parts Homebrew cannot:

```sh
curl -fsSL https://raw.githubusercontent.com/RoleModel/rolemodel-openscreen/main/install.sh | sh
```

## This repository is a build output

**Do not edit these files here.** They live in
[`packaging/`](https://github.com/RoleModel/rolemodel-openscreen/tree/main/packaging)
in the toolkit repo, and `npm run sync-tap` there copies them across. `npm run
check` fails if the two drift.

This repo exists only because Homebrew resolves `brew tap rolemodel/tap` to a
repository named `homebrew-tap` and nothing else. That naming rule is the entire
reason it is separate — it is not a place to work.

| file | is |
|---|---|
| `Formula/rm-video.rb` | the toolkit: eight CLIs and the `openscreen` shim |
| `Casks/rolemodel-openscreen.rb` | our fork of the app |
| `scripts/update-cask.mjs` | points a cask at a release, with checksums from the real installers |

## Cutting a release

```sh
node scripts/update-cask.mjs rolemodel-openscreen v1.9.6-rm.2
```

That reads the release's assets, hashes them, and rewrites the version and both
checksums. Hand-editing them is how a tap ends up installing last month's build —
upstream ships an `update-homebrew-cask.yml` that has never been configured
(their issue #335), and the job it guards has been skipping green on every
release since. The failure is silent, which is the problem.
