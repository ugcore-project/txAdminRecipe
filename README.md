<div align="center">

# ⬢ UgCore txAdmin Recipe

**Deploy a ready UgCore server from txAdmin in one step.**

[![Documentation](https://img.shields.io/badge/Docs-ugcore.urging.ch-7C5CFF?logo=readthedocs&logoColor=white)](https://ugcore.urging.ch)
[![License](https://img.shields.io/github/license/ugcore-project/txAdminRecipe?color=brightgreen)](LICENSE)
[![Discord](https://img.shields.io/discord/1518308384900649021?label=Discord&logo=discord&logoColor=white&color=5865F2)](https://discord.gg/pCJUcyVw5X)

</div>

---

## Deploy

1. In txAdmin, open the **Server Deployer** and pick **Remote URL Template**.
2. Paste:

   ```text
   https://raw.githubusercontent.com/ugcore-project/txAdminRecipe/main/ugcore.yaml
   ```

3. Review the recipe. Change `gameBuild`, or delete a resource block to leave it out.
4. Enter your Cfx license key and the database. txAdmin creates the database when it does not exist.
5. Run it, then start the server. ug-core prints its banner, creates its tables and generates its config.

Requirements: OneSync (the recipe sets it), MySQL 8.0+ or MariaDB 10.6+.

## What you get

```text
server.cfg          network, slots, game build, then runs the files below in order
secrets.cfg         license key and database. Never share or commit it
convars.cfg         FXServer hardening, Steam, oxmysql, pma-voice, ug-core and ug-lib settings
permissions.cfg     ug-core's ACEs and your staff
resources.cfg       the resources that start, in order
.gitignore          keeps secrets.cfg and runtime files out of git
resources/
├── [cfx]/          mapmanager, spawnmanager (chat is built into FXServer)
├── [standalone]/   oxmysql, pma-voice
├── [ug]/           ug-lib, ug-core, ug-multicharacter, ug-appearance, ug-hud, ug-target, ug-inventory, ug-admin
└── [local]/        your own resources
```

- **Latest releases.** Every ug resource and oxmysql come from their latest release archive, never from a branch.
- **Settings in one place.** Every ug-core and ug-lib option that is not a table is a convar in `convars.cfg`, set
  to its default: module switches, Guard thresholds, character slots, inventory size, UI positions, theme. Convars
  win over the generated `config/` files.
- **Secrets in one file.** `secrets.cfg` is the only file to protect. Everything in it uses `set`, so nothing
  reaches clients or the server list.
- **Least privilege.** ug-core gets the four ACE commands it runs (`add_ace`, `remove_ace`, `add_principal`,
  `remove_principal`), not `command`.
- **Hardened by default.** `sv_entityLockdown relaxed`, `sv_filterRequestControl 1`, no networked sounds, no Script
  Hook, private endpoints, `sv_pureLevel 1`. Each line says what it does and when to change it.
- **You are admin.** The txAdmin master account is added to `group.admin`, which has `ug.admin`.
- **No basic-gamemode.** It turns spawnmanager auto-spawn back on. ug-core owns spawning.
- **Categories.** `ensure [ug]` starts every UgCore resource, so removing one never breaks the start.

## Variables

| Variable | Default | Sets |
| --- | --- | --- |
| `gameBuild` | `3407` | `sv_enforceGameBuild`: the game build clients run. |

txAdmin also fills `svLicense`, `dbConnectionString`, `serverEndpoints`, `maxClients`, `serverName` and
`addPrincipalsMaster`.

## Updating

Download the new release of a resource and extract it over its folder in `resources/[ug]`. Its `config/` ships empty,
so your config stays. Restart the server: ug-core runs new migrations on start.

## Development

The cfg files live in [`server-data/`](server-data). The recipe downloads that folder from `main`, so a change reaches
new deployments as soon as it is merged. `gitignore` is copied to `.gitignore` in the server folder; it has no dot here
so it does not apply to this repository.

Every ug resource MUST attach a version-less `<name>.zip` to its releases: the recipe reads
`releases/latest/download/<name>.zip`.

## License

Licensed under the [GNU General Public License v3.0](LICENSE).

Copyright (C) 2026 UgCore Framework & Contributors.
