# BossModRebornButBetter

An unofficial fork of [BossMod Reborn](https://github.com/FFXIV-CombatReborn/BossmodReborn) by The Combat Reborn
Team. All credit for the plugin goes to the original authors. This repo is not affiliated with or supported by them;
please don't report issues with this fork upstream.

## Why this fork exists

The official plugin only runs the boss modules built into it. This fork is an automated build with a small patch
that loads extra boss module DLLs from `%APPDATA%\XIVLauncher\pluginConfigs\BossModReborn\CustomModules\`, so custom
versions of encounters can be maintained separately instead of in a full fork of the plugin. Everything else is
unchanged.

A custom module assembly must be named `BossModCustomModules` and be compiled with BossMod's `BossMod.SourceGen`
analyzer. Its modules replace built-in modules with the same full type name or primary actor OID, and its config
nodes replace built-in ones with the same full type name, so existing settings and plans carry over.

## How it works

`.github/workflows/release.yml` runs every 4 hours (and on changes to `patches/`):

1. Finds the newest upstream release tag (e.g. `7.5.6.28`).
2. Clones that tag and applies `patches/*.patch` with `git am`.
3. Builds as version `<major>.<minor>.<build>.<revision*100 + PATCH_REVISION>` (e.g. `7.5.6.2801`).
4. Publishes a GitHub release and updates `repo.json`.

If the patches stop applying, the run fails and GitHub notifies you.

## Updating the patch

Regenerate with `git format-patch -o patches/ <upstream-tag>..<patch-branch>` and bump `PATCH_REVISION`, otherwise
Dalamud won't see an update.

## Install

1. Uninstall the official BossMod Reborn (same internal name; settings are kept).
2. Dalamud Settings > Experimental > Custom Plugin Repositories, add
   `https://raw.githubusercontent.com/IndividualGather/BossModRebornButBetter/main/repo.json`.
3. Install "BossModRebornButBetter".
