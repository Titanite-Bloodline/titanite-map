# Titanite Map

Titanite Map is the Titanite SMP's fork of [squaremap](https://github.com/jpenilla/squaremap) 1.4.3, the live web map for Minecraft servers. It runs on Paper 26.3 with Java 25.

## What this fork changes

The `titanite` branch starts at upstream tag `v1.4.3`. Only these things differ, so upstream fixes still merge cleanly.

- **Plugin name.** The plugin is called `TitaniteMap` and declares `provides: [squaremap]`, so plugins that depend on squaremap still load. The Java packages and the public API (`SquaremapProvider.get()`, `PlayerManager`, the layer registry) are unchanged. The command, the permissions and the log prefix still say `squaremap`. The data folder is `plugins/TitaniteMap`, so an old `plugins/squaremap` folder is not picked up.
- **Players start hidden.** A player shows on the map only after something calls `PlayerManager.show(uuid, ...)` for them. This is the default inside the player manager, so if Titanite is missing or fails, nobody shows. `hide`, `show` and `hidden` work as before through the API and through `/squaremap hide` and `/squaremap show`. Shown status lasts until the server restarts or `/squaremap reload`, even when `show` is called with `persistent` set to true: a persistent show is not stored, so a missing Titanite cannot leave dots on the map. A persistent `hide` is still stored and still beats a show.
- **Web page.** The page title is `Titanite Map` (the tab reads `Titanite Map - <world>` once the map loads) and the favicon is the Titanite Minecraft mark. The link preview titles say `Titanite Map`.
- **Jar name.** The jar is named `TitaniteMap-<version>.jar` (for example `TitaniteMap-1.4.3-titanite.1.jar`). The version is the upstream version plus `-titanite.N`.

Not changed, so you know: the update checker still asks Modrinth about upstream releases and reports this fork as out of date (`settings.update-checker: false` in `config.yml` turns it off). The bStats call still reports under squaremap's id. The Fabric, NeoForge and Sponge modules are untouched and not built here, so the shared player manager change is untested on them.

## Licence

squaremap is MIT and so is this fork. The [LICENSE](LICENSE) file keeps the original copyright (Jason Penilla and contributors).

## Building

You need JDK 25 and [Bun](https://bun.sh/), which builds the web front end. Run `bun install` once inside `web/`, then build only the Paper platform:

```
./gradlew :squaremap-paper:assemble
```

The jar lands in `build/libs/`. Set the web server port in `plugins/TitaniteMap/config.yml` under `settings.internal-webserver.port` (the default is 8080).

## The upstream README

What follows is the upstream README.

<div align="center">

# squaremap

[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/jpenilla/squaremap/build.yml?branch=master)](https://github.com/jpenilla/squaremap/actions)
[![Discord](https://img.shields.io/discord/390942438061113344?color=8C9CFE&label=discord&logo=discord&logoColor=white)](https://discord.gg/PHpuzZS)

</div>

## What is squaremap

squaremap is a minimalistic and lightweight live world map viewer for Minecraft servers.

squaremap hooks into your Minecraft server as a plugin or mod on a [supported platform](#supported-platforms), then generates and manages a live map of your server, viewable in any web browser.

## Features

* Ultra fast render times. Get your map rendered today, not next week.
* Simple vanilla-like top down 2D view, designed for navigation.
* Player markers showing yaw rotation, health, and armor.
* Easy to [setup](https://github.com/jpenilla/squaremap/wiki/Installation) and [configure](https://github.com/jpenilla/squaremap/wiki/Default-config.yml).
* Up to date Leaflet front-end.
* [Addons and integrations](https://github.com/jpenilla/squaremap/wiki/Addons) for many popular plugins.

## Supported platforms

- [Paper](https://papermc.io/)
- [Fabric](https://fabricmc.net/) (requires [Fabric API](https://modrinth.com/mod/fabric-api))
- [NeoForge](https://neoforged.net/)
- [Sponge](https://www.spongepowered.org/)

## Downloads

Downloads can be obtained from [Modrinth](https://modrinth.com/mod/squaremap).

<details>
<summary>Development builds</summary>

> Development builds are available at https://jenkins.jpenilla.xyz/job/squaremap/
</details>

## Demo

Official squaremap demo: https://squaremap-demo.jpenilla.xyz/

## License

This project is licensed under the [MIT license](https://github.com/jpenilla/squaremap/blob/master/LICENSE)

Leaflet (the web ui frontend) is licensed under [2-clause BSD License](https://github.com/Leaflet/Leaflet/blob/master/LICENSE)

## API

squaremap provides simple APIs to draw markers, shapes, icons, and etc. on rendered maps. Javadocs are hosted on the maven repository alongside the binaries, and should be automatically downloaded by your IDE. 

### Dependency Information

Releases are published to Maven Central

<details>
<summary>Using snapshot builds</summary>

> Snapshot builds are available on the Sonatype snapshots maven repository: `https://central.sonatype.com/repository/maven-snapshots/`
>
> Consult your build tool's documentation for details on adding maven repositories to your project.
</details>

Maven
```xml
<dependency>
    <groupId>xyz.jpenilla</groupId>
    <artifactId>squaremap-api</artifactId>
    <version>1.4.3</version>
    <scope>provided</scope>
</dependency>
```

Gradle
```kotlin
repositories {
    mavenCentral()
}

dependencies {
    compileOnly("xyz.jpenilla", "squaremap-api", "1.4.3")
}
```

## Develop

### Prerequisites and setup
- An up-to date JDK (i.e. Java 21 or later)
- [Bun](https://bun.sh/) installed for building the web UI
- Run `bun install` in the `web` directory to install the web UI dependencies

### Plugin

Build squaremap by invoking the `build` task with Gradle.

```
./gradlew build
```

### Web UI

The web UI will be automatically built and included in the plugin jar by Gradle when building the plugin.

When using the various run tasks (i.e. `:squaremap-paper:runServer`), Gradle will pass the
`squaremap.devFrontend=true` and `squaremap.frontendPath=<absolute path to ./web>` system properties to
the server. When these properties are present, the plugin will attempt to run the Vite dev server and
proxy requests to it. This allows hot module reloading to work when developing the web UI.

## bStats

Usage stats for the Paper platform

[![bStats Graph Data](https://bstats.org/signatures/bukkit/squaremap.svg)](https://bstats.org/plugin/bukkit/squaremap/13571)
