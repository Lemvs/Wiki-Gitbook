---
description: >-
  Schematics are used as a default building of islands (no one wants an empty
  world to play inside). There are three different schematics: normal ones,
  nether ones & end ones.
---

# Schematics

## Creating your first schematic

Here you can find a tutorial for using the built-in system - which is very fast and optimized to ensure no lag is occured. If you prefer to use WorldEdit schematics, please scroll down for the WorldEdit section.

First, toggle the schematic mode using /is admin schematic.\
Then, select two blocks using a golden axe, and stand where you want the teleport location to be.\
The last thing to do is to run /is admin schematic , which will create the schematic file for you.\
This task might be a bit slow, so don't do it while having many players online.

**Creating a sign**\
To create a sign that will appear on the island - you have two options:

1\. You can create a sign when you create the schematic\
2\. You can place a sign on the island when you create a schematic and edit the `default-signs` option in the `config.yml`.

For both, you can use these variables:\
`{player}` - Island Owner's Name\
`{island}` - Island Name _(If the island name does not exist, then it will default to the Island Owner's Name.)_

{% embed url="https://static.bg-software.com/imgs/schematics-creation.mp4" %}

{% hint style="info" %}
By default, air blocks are skipped when saving a schematic. If you want to include them in schematic, use `/is admin schematic <schematic-name> true` when saving.
{% endhint %}

## WorldEdit Schematics

WorldEdit schematics are supported, but you must have FastAsyncWorldEdit installed.\
Support for regular WorldEdit won't be added, as FAWE is the same but better in performance.

{% hint style="danger" %}
WorldEdit schematics do not receive bug-fixes anymore.\
Any bug from using these schematics will not be fixed, and it's recommended to use the built-in system instead.
{% endhint %}

## Adding your own schematic

In order to add schematics, you first need to move the schematic files into the schematics folder,\
&#x20;that is located under the main plugin's folder. Make sure you use WorldEdit schematics or built-in schematics.

After doing so, allocate the island-creation menu file (can be found inside the menus folder), and open it.\
Add a new item to the items section, and add to it the following sections:\
`schematic`: '' - the name of the schematic file (without file extension)\
`biome`: '' - The biome that will be set for the island.\
`bonus-worth`: - The bonus that the island will get of worth once created.\
&#xNAN;_&#x43;an be negative to get the island start with a default worth of 0._\
`bonus-level`: - The bonus that the island will get of level once created.\
&#xNAN;_&#x43;an be negative to get the island start with a default level of 0._\
`offset`: - When enabled, created islands will have a worth & level values of 0.\
This function checks for the default worth of the island, and applies a negative worth bonus.\
`spawn-offset`: - Offset to teleport the player when the island is created.\
&#xNAN;_&#x54;he format of the offset is "\<offset-x>, \<offset-y>, \<offset-z>"_\
`access`: - The item that will be displayed when having access to the schematic.\
`no-access`: - The item that will be displayed when not having access to the schematic.
