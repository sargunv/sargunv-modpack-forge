# Example MultiMC modpack using Modsman

This repo is an example and starting point for putting together a modpack that can be loaded in MultiMC using [Modsman](https://github.com/sargunv/modsman/) to download content like mods and resource packs instead of distributing them directly. This readme contains information on what the experience is like for users, usage instructions for modpack creators, and details on how it works under the hood.

This example pack consists of all my mods, their dependencies, and the Faithful texture pack. Import this URL into MultiMC to check it out for yourself:

```
https://github.com/sargunv-mc-mods/modsman-modpack-example/archive/master.zip
```

## Users' experience

Users don't need to know anything about Modsman. To them, the pack works like any other MultiMC instance.

1. Download the modpack zip distributed by the pack creator
2. Import the modpack zip into MultiMC
3. Launch the game
4. On first run, the included prelaunch script will download modsman-packutil and invoke it to download mods and resource packs from CurseForge
5. On subsequent runs, Modsman will NOT be invoked again, just like a regular pack

## User guide for modpack creators

Some knowledge of Modsman and MultiMC instances is required. Modsman is pretty straightforward to figure out if you're already familiar with using the command line on your platform.

1. Clone this repo, or [create a new repo using this as a template](https://github.com/sargunv-mc-mods/modsman-modpack-example/generate)
2. [Install modsman via Scoop or Homebrew](https://github.com/sargunv/modsman/blob/master/README.md)
3. Set up your pack inside this instance, ensuring files from CurseForge are tracked in the *.modlist.json*
   * see the [Modsman README.md](https://github.com/sargunv/modsman/) for usage instructions
4. Optionally, edit the `PreLaunchCommand` in *instance.cfg* to add `-i` flags for subdirectories you want Modsman to be invoked
   * by default, this is *mods* and/or *resourcepacks*
5. Create your pack zip **EXCLUDING** the mod jars/zips tracked by *.modlist.json* and the *.MODSMAN_FIRST_RUN_COMPLETE* files but **INCLUDING** the actual *.modlist.json* in those directories, as well as the prelaunch class and scripts
   * make sure the `PreLaunchCommand` in *instance.cfg* is preserved too!
6. Distribute the pack zip to users after testing in MultiMC

## How it works

This system is inspired by the technique used in the [AOF 3 by AK9](https://github.com/AllOfFabric/All-of-Fabric-3) pack.

 * MultiMC includes a `PreLaunchCommand` setting which allows us to set up a program to run before Minecraft runs.
 * The *modsman-packutil* program is a specialized version of Modsman which will invoke the equivalent of `modsman-cli reinstall-all` in all the directories specified by the `-i` flags in the command.
 * The included prelaunch scripts download the *modsman-packutil* program at runtime if not present, and then execute it to invoke modsman.
