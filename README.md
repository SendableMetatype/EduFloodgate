# EduFloodgate

A [Floodgate](https://github.com/GeyserMC/Floodgate) fork that adds **Minecraft Education Edition** player support for online mode Java servers. Companion plugin for [EduGeyser](https://github.com/SendableMetatype/EduGeyser).

## Features

- **Education player identity** - Deterministic UUID generation for edu players who lack Xbox Live accounts
- **Tenant-aware usernames** - Usernames formatted as prefix + name + 4-char tenant hash (e.g. `#john7a3f`) to distinguish players across schools
- **FloodgatePlayer API** - `isEducationPlayer()`, `getTenantId()`, and `getAdRole()` for downstream plugins
- **Xbox Live linking bypass** - Education clients are automatically excluded from player linking (no Xbox account to link)
- **BedrockData protocol extension** - Education fields (isEdu, tenantId, adRole) passed through the Floodgate data pipeline

## Downloads

Pre-built jars are available on the [Releases](https://github.com/SendableMetatype/EduFloodgate/releases) page.

Requires [EduGeyser](https://github.com/SendableMetatype/EduGeyser).

## Documentation

- **[Setup Guide](https://github.com/SendableMetatype/EduGeyser/blob/full/SETUP-GUIDE.md)** - How to install and configure EduGeyser + EduFloodgate

## How It Works

Standard Floodgate generates player UUIDs from Xbox Live XUIDs. Education Edition players have no Xbox account, so EduFloodgate uses an alternative identity scheme:

1. **UUID generation**: SHA-256 hash of `tenantId:username`, with MSB set to `0x0000000100000001` to avoid collisions with both Java random UUIDs and standard Floodgate XUID-based UUIDs
2. **Username formatting**: Configurable prefix (default `#`) + player name + 4-character hex hash of the tenant ID, keeping usernames unique across schools
3. **Data flow**: EduGeyser extracts education fields from the client's EduTokenChain JWT during login, encodes them in the BedrockData payload, and EduFloodgate reads them during the handshake to set up the player correctly

The `isFloodgateId()` check recognizes both standard (MSB `0`) and education (MSB `0x0000000100000001`) UUID formats.

---

# Floodgate

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Build Status](https://github.com/GeyserMC/Floodgate/actions/workflows/build.yml/badge.svg?branch=master)](https://github.com/GeyserMC/Floodgate/actions/workflows/build.yml?query=branch%3Amaster)
[![Discord](https://img.shields.io/discord/613163671870242838.svg?color=%237289da&label=discord)](http://discord.geysermc.org/)
[![Hits](https://hitcount.dev/p/GeyserMC/Floodgate.svg)](https://hitcount.dev/p/GeyserMC/Floodgate)

[Download](https://geysermc.org/download/?project=floodgate)

Hybrid mode plugin to allow for connections from [Geyser](https://github.com/GeyserMC/Geyser) to join online mode servers.

Geyser is an open collaboration project by [CubeCraft Games](https://cubecraft.net).

See the [Floodgate](https://geysermc.org/wiki/floodgate/) section in the GeyserMC Wiki for more info about what Floodgate is, how you setup Floodgate and known issues/caveats. Additionally, it includes a more in-depth look into how Floodgate works and the Floodgate API.
