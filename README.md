# Seed-splitter
Perfect SSR seed finding based on map spliting

### MineAChunkSeedfinding

In this project, I use KaptainWutax's seedutils to find good seeds to run for Minecraft 1.16.1 Set-Seed Mine-A-Chunk speedruns.

My group usually runs co-op, but these seeds can be used for singleplayer aswell.


## Installation
1. Create a new IntelliJ project, with gradle support. 
2. Either use my build.gradle, or add the following to yours: 
```
repositories {
    mavenCentral()
    maven {
        url "https://jitpack.io/"
    }
}
dependencies {
    implementation 'com.github.KaptainWutax:SEED:master-SNAPSHOT'
}
```
3. Clone the src folders from this project into your IntelliJ project.
That's it! 

## Explaination
To understand this, you first need to understand the difference between Structure seeds and World seeds.\
(Things are more complicated than this, but for this oroject, this is all you need to know).\
A structure seed is the lower 48 bits of a world seed, and determines the structures that are spawned, as well as some other things including ravines.\
We use this to iterate over all 2^48 structure seeds, and find good ones where two wide ravines intersect eachother, with small angle difference.\
This maximises the amount of blocks taken out by the ravines.\
Then, once we have good structure seeds, we can generate a good world seed with it by iterating through the lower 16 bits.\
This (together with the structure seed) determines the biome, spawnpoint and some other factors.\
We try to get a good world seed so that we spawn in a forest, with a spawnpoint close to where the two ravines intersect, and the intersection point is not underwater.


## Usage
To generate structure seeds, you can leave the viableStructureSeeds array in main.java empty. Then, it will spawn a number of threads, default 12, to iterate over all seeds sequentially\
and try to find good ones. \
It will print these out to the console, this will be the output, in the format {structureseed, intersectx, intersectz}.\
It will also print how far it has come, every 1.000.000.000 seeds. This can be used to update the start variable so that you dont iterate over the same seeds multiple times.\
If you copy all the structureseeds (in the array-format that they are) into viableStructureSeeds and run the program again, it will go onto the next step:\
Finding a good world seed for that structure seed.\
It will print world seeds and intersection coordinates to the console, and then you can go into a world with that seed to see if it's any good.


Our current best chunk is in seed: 4998434179546958649\
x: 26-41\
z: 34-49\
which only has about 3687 blocks that need to be mined.

# Seed Chunk Checker

Checks the frequencies of blocks in a given chunk of a given seed.


## Usage
1. Build shadow (fat) jar with `./gradlew shadowJar` (`gradlew.bat shadowJar` on windows)
2. Move the file at `build/libs/seed-chunk-checker-0.1.0-all.jar` to that folder `server/`.
3. Rename the jar you just moved to `server.jar`
5. Add your seeds to `seeds.txt`. (See accepted seed formats).
6. Run `python3 main.py` to start generating worlds and finding optimal chunks.
7. Run `python3 analyze.py` to analyze your results.

### Accepted seed formats
The program can handle the following formats for seeds (One per line):  
`seed`  
`seed: x,y`  

# SeedCracker
## Installation

 ### Vanilla Launcher

  Download and install the [fabric mod loader](https://fabricmc.net/use/).

 ### MultiMC

  Add a new minecraft instance and press "Install Fabric" in the instance options.

## Usage

  Run minecraft with the mod installed and run around in the world. Once the mod has collected enough data, it will start the cracking process automatically and output the seed in chat. For the process to start, the amount of data that needs to be collected varies depending on the type of feature. `/seed data bits` can be used to see how much progress has been done. 
  
  ### Supported Structures
    - Ocean Monument
    - End City
    - Buried Treasure
    - Desert Pyramid
    - Jungle Temple
    - Swamp Hut
    - Shipwreck
  
  ### Supported Decorators
    - Dungeon
    - End Gateway
    - Desert Well
    - Emerald Ore

## Commands

  The command prefix for this mod is /seed.
  
  ### Render Command  
  -`/seed render outlines <ON/OFF/XRAY>`
    
  This command only affects the renderer feedback. The default value is 'XRAY' and highlights data through blocks. You can set    the render mod to 'ON' for more standard rendering. 
  
  ### Finder Command
  -`/seed finder type <FEATURE_TYPE> (ON/OFF)`
  
  -`/seed finder category (BIOMES/ORES/OTHERS/STRUCTURES) (ON/OFF)`
  
  This command is used to disable finders in case you are aware the data is wrong. For example, a map generated in 1.14 has different decorators and would require you to disable them while going through those chunks.

  ### Data Command
  - `/seed data clear`
  
  Clears all the collected data without requiring a relog. This is useful for multi-world servers.
  
  - `/seed data bits`
  
  Display how many bits of information have been collected. Even though this is an approximate, it serves as a good basis to guess when the brute-forcing should start.
  
  ### Cracker Command
  - `/seed cracker <ON/OFF>`
 
  Enables or disables the mod completely. Unlike the other commands, this one is persistent across reloads.



## Building the Mod

-Update the version in `build.gradle` and `fabric.mod.json`.

-Run `gradlew build`.



