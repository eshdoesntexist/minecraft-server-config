# My Minecraft Server config

This repository contains a small server configuration used to run a modded Minecraft server which is close to vanilla and mostly covers performance mods inside Docker using the `itzg/minecraft-server` image and a Modrinth `.mrpack` modpack file (`server_modpack.mrpack`). This README documents the docker-compose extracts, important environment variables, custom commands added by mods, and the server mod list.

# extract of docker config for the server

```yaml
esh_mc:
  image: itzg/minecraft-server
  tty: true
  stdin_open: true
  restart: unless-stopped
  volumes:
    - [name of volume]:/data # replace with the name of the volume you wish to use for the server
    #TODO use github to dynamically load it in the future so that modpack can be much more easily updated
    - type: bind
      source: /root/modpacks/ # for now i just store the modpack on the server. 
      target: /modpacks
  environment:
    EULA: "TRUE"
    ONLINE_MODE: false # a lot of people use tlauncher so yeah. Added skin restorer so that people can use skins
    OPS: "eshdoesntexist" # my minecraft username. change it with yours if you plan to use this
    SEED: 7830518263619659759
    DIFFICULTY: "hard"
    USE_AIKAR_FLAGS: true
    VIEW_DISTANCE: 24
    SIMULATION_DISTANCE: 6
    MEMORY: 6G
    TYPE: MODRINTH # refer to https://docker-minecraft-server.readthedocs.io/en/latest/types-and-platforms/mod-platforms/modrinth-modpacks/
    MODRINTH_MODPACK: "server_modpack.mrpack"
    MODRINTH_DOWNLOAD_DEPENDENCIES: true
    VERSION: 1.21.8 # i have the version set to a specific one for now. i will update this in the future as mods are updated.
```

currently i also use the mc router since i have multiple servers hosted. this leads to issues such as being unable to ip ban (needed as people with tlauncher can easily circumvent username based bans). in the future i plan to use velocity to allow that as well.
the extract for which is as follows:

```yaml 

 mc_router:
    image: itzg/mc-router
    depends_on:
      - esh_mc
      # ... add names of other mc containers that need to be mapped
    environment:
      MAPPING: |
        [the server domain]=esh_mc:25565 # dont forget to replace  [the server domain] with the actual server domain
        # ... other mappings go here
    ports:
      - "25565:25565"
```

# custom commands available

the mods enable following commands:

| Command                   | Description                                                                                    | Mod Name                                                |
| ------------------------- | ---------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| `/tpa [player_name]`      | Send a teleport request to the player. If accepted, you will be teleported. Expires after 60s. | [TPA Utilities](https://modrinth.com/mod/tpa-utilities) |
| `/tpahere [player_name]`  | Send a teleport request for the player to teleport to you. Expires after 60s if not accepted.  | [TPA Utilities](https://modrinth.com/mod/tpa-utilities) |
| `/tpaccept`               | Accept the last teleport request sent to you.                                                  | [TPA Utilities](https://modrinth.com/mod/tpa-utilities) |
| `/tpadeny`                | Refuse the last teleport request sent to you.                                                  | [TPA Utilities](https://modrinth.com/mod/tpa-utilities) |
| `/tpacancel`              | Cancel all your pending teleport requests.                                                     | [TPA Utilities](https://modrinth.com/mod/tpa-utilities) |
| `/tpalock`                | Enable/Disable the ability of other players to send you teleport requests.                     | [TPA Utilities](https://modrinth.com/mod/tpa-utilities) |
| `/tpalanguage [language]` | Change the language used by the mod for the player.                                            | [TPA Utilities](https://modrinth.com/mod/tpa-utilities) |

# mods list

- [Servux](https://modrinth.com/mod/servux)
- [FerriteCore](https://modrinth.com/mod/ferrite-core)
- [Skin Restorer](https://modrinth.com/mod/skinrestorer)
- [TPA Utilities](https://modrinth.com/mod/tpa-utilities)
- [Cloth Config API](https://modrinth.com/mod/cloth-config)
- [Player Drops Head](https://modrinth.com/mod/player-drops-head)
- [Very Many Players (Fabric)](https://modrinth.com/mod/vmp-fabric)
- [ScalableLux](https://modrinth.com/mod/scalablelux)
- [Fabric API](https://modrinth.com/mod/fabric-api)
- [Concurrent Chunk Management Engine (Fabric)](https://modrinth.com/mod/c2me-fabric)
  [Lithium](https://modrinth.com/mod/lithium)
