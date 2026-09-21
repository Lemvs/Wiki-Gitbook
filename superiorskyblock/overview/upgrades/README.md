---
description: >-
  Using upgrades, you can give your players custom perks that are upgradable. In
  this documentation, you will understand how to edit them to your own style!
---

# Upgrades

## The Upgrades Module

Upgrades are part of the upgrades module of the plugin. Therefore, they are configured in the config file of the module, located in `plugins/SuperiorSkyblock2/modules/upgrades/config.yml`, and not in the main config.yml of the plugin.

{% hint style="info" %}
If you have an old `upgrades.yml` file from older versions of the plugin, it will be migrated automatically into the module's config file.
{% endhint %}

Besides the upgrades themselves, the config file of the module contains the following global settings:

| Field            | Default | Description                                                                                    |
| ---------------- | ------- | ---------------------------------------------------------------------------------------------- |
| `enabled`        | true    | Whether the module should be enabled. When disabled, all module commands and features will also be disabled. |
| `crop-growth`    | true    | Whether crop-growth should be enabled. When disabled, the plugin will not alter crop growth.   |
| `mob-drops`      | true    | Whether mob drops should be enabled. When disabled, the plugin will not alter mob drops.       |
| `island-effects` | true    | Whether island-effects should be enabled. When disabled, the plugin will not give any effects to any players. |
| `spawner-rates`  | true    | Whether spawner-rates should be enabled. When disabled, the plugin will not alter spawner rates. |
| `block-limits`   | true    | Whether block-limits should be enabled. When disabled, the plugin will not limit placement of blocks. |
| `entity-limits`  | true    | Whether entity-limits should be enabled. When disabled, the plugin will not limit spawning of entities. |

## Creating your first upgrade

Creating a new upgrade is an easy task to do!\
All the upgrades follow the same rules, but in this tutorial we will create a generator upgrade.

We will start with the basic layout for the upgrade:

```yaml
upgrades:
  island-generators:        # The name of the upgrade.
    '1':                    # The first level of the upgrade.
      <to-do>
    '2':                    # The second level of the upgrade.
      <to-do>
```

All the upgrades work with the same layout: a global section for the upgrade, and sub-sections for every level of the upgrade. You can make as many levels as you want!

Now, we can start working on our first level. We will give it a price-type, price, commands to be executed upon rankup and a required permission to use the upgrade. In this case, I don't want a permission - so I don't add that section.

```yaml
'1':
  price-type: 'money'      # The type of price handler. More information below.
  price: 100000.0          # The price to rank up to the second level.
  commands:
    - 'island admin setupgrade %player% island-generators 2'     # We must change the level of the upgrade manually using a command.
    - 'island admin msgall %player% &e&lUpgrade | &7%player% upgraded your generators to level 2!'   # Message that will be sent to the island members.
  permission:  <your-permission>   # You can add it if you want a required permission to rankup.
```

As you might have noticed, I run /is admin setupgrade - this is required so the plugin will actually change the upgrade's level for the island. Not doing so will make the upgrade to not rankup, as you'll see in the last level.

After we set up the basic layout of the level, we want to give it some values. The values will be synced with the island. You can change crop growth, spawner rates, mob drops, limits, generators and more with the upgrades! For this tutorial, I will change the generator rates using the "generator-rates" section:

```yaml
'1':
 price-type: 'money'
 price: 100000.0
 commands:
   - 'island admin setupgrade %player% island-generators 2'
   - 'island admin msgall %player% &e&lUpgrade | &7%player% upgraded your generators to level 2!'
 generator-rates:
   normal:      # The world environment. Can use normal, nether or the_end.
     STONE: 85
     COAL_ORE: 10
     IRON_ORE: 5 
```

That's it! We completed our first level! Because this is the first level of the upgrade, it will be applied to all the islands by default. It means that all of the islands on my server will have the generator rates that I configured. Now, I will add more levels by following the same layout:

```yaml
upgrades:
 island-generators:
   '1':
     price-type: 'money'
     price: 100000.0
     commands:
       - 'island admin setupgrade %player% island-generators 2'
       - 'island admin msgall %player% &e&lUpgrade | &7%player% upgraded your generators to level 2!'
     generator-rates:
       normal:
         STONE: 85
         COAL_ORE: 10
         IRON_ORE: 5 
   '2':
     price-type: 'money'
     price: 150000.0
     commands:
       - 'island admin setupgrade %player% island-generators 3'
       - 'island admin msgall %player% &e&lUpgrade | &7%player% upgraded your generators to level 3!'
     generator-rates:
       normal:
         STONE: 70
         COAL_ORE: 15
         IRON_ORE: 10
         DIAMOND_ORE: 5
   '3':
     price-type: 'money'
     price: 300000.0
     commands:
       - 'island admin setupgrade %player% island-generators 4'
       - 'island admin msgall %player% &e&lUpgrade | &7%player% upgraded your generators to level 4!'
     generator-rates:
       normal:
         STONE: 50
         COAL_ORE: 25
         IRON_ORE: 15
         DIAMOND_ORE: 10
```

After I configured all of my levels, I must also add the last upgrade - level #4. Unlike the other upgrades, this upgrade will not have the setupgrade command, but will still have values assigned to it:

```yaml
'4':
  price-type: 'money'
  price: 0.0       # I set the price to 0, so my players will always get the warning message.
  commands:
    - 'island admin msg %player% &c&lError | &7You have reached the maximum upgrade for island generators.'
  generator-rates:
    normal:
      EMERALD_ORE: 50
      DIAMOND_ORE: 50
```

Finally, I have a working generator upgrade that will have it's values synced with all the islands. You can change the values anytime you want, and your islands will be synced automatically with it. Removing existing levels is not an option - you can just make the upgrade to do nothing, but removing it completely will cause errors from the plugin.

## Level Fields

Every level of an upgrade can have the following fields:

| Field             | Type   | Description                                                                                                              |
| ----------------- | ------ | ------------------------------------------------------------------------------------------------------------------------ |
| `commands`        | List   | Commands that will be executed by the console when the level is purchased. You can use `%player%` for the player's name. |
| `permission`      | String | Optional permission that is required to rankup to the configured level.                                                  |
| `required-checks` | List   | Optional conditions that is required to rankup to the configured level. More information below.                          |

Each level can also have a price or multiple prices, which you will learn more about below.

{% hint style="info" %}
Unlike prices, where the rankup cost is taken from the current level, permissions and required-checks are taken from the next level, so start configuring them from level 2.
{% endhint %}

### Required Checks

Using the `required-checks` field, you can add custom conditions that players must meet before they can rankup to the configured level. Each entry in the list is in the format `<condition>;<error-message>`: the condition is evaluated by the [JavaScript engine](../javascript-engine.md) (placeholders are supported), and if it's not met, the error message will be sent to the player.

For example, requiring the island to be at least level 10 in order to purchase the level:

```yaml
'2':
  price-type: 'money'
  price: 150000.0
  required-checks:
    - '%superior_island_level% >= 10;&cYour island must be level 10 or higher to purchase this upgrade!'
  commands:
    - ...
```

## Island Values

You can use the following sections to alter island values:

| Field             | Type    | Description                                                                                                                                                                                                                                                     |
| ----------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `crop-growth`     | Double  | The crop growth multiplier for this upgrade.                                                                                                                                                                                                                    |
| `spawner-rates`   | Double  | The spawner rates multiplier for this upgrade.                                                                                                                                                                                                                  |
| `mob-drops`       | Double  | The mob drops multiplier for this upgrade.                                                                                                                                                                                                                      |
| `team-limit`      | Integer | The team limit for this upgrade.                                                                                                                                                                                                                                |
| `warps-limit`     | Integer | The warps limit for this upgrade.                                                                                                                                                                                                                               |
| `coop-limit`      | Integer | The coops limit for this upgrade.                                                                                                                                                                                                                               |
| `border-size`     | Integer | The border size for this upgrade. Must not exceed the `max-island-size` from the main config, otherwise the level will be skipped.                                                                                                                              |
| `bank-limit`      | String  | The maximum amount of money that can be deposited into the island bank for this upgrade. Supports large numbers.                                                                                                                                                |
| `block-limits`    | Section | The block limits for this upgrade. All the blocks are in the format `TYPE: LIMIT`. Block types also support data values, in the format `TYPE:DATA`.                                                                                                             |
| `entity-limits`   | Section | The entity limits for this upgrade. All the entities are in the format `TYPE: LIMIT`.                                                                                                                                                                           |
| `generator-rates` | Section | The generator rates for this upgrade. The rates are configured per world environment (`normal`, `nether` or `the_end`), with all the rates in the format `TYPE: CHANCE`. Rates that are placed directly under the section (legacy format) will apply to the default world of the plugin. |
| `island-effects`  | Section | The island effects for this upgrade. All the effects are in the format `EFFECT: LEVEL`, where the level is the in-game effect level (`SPEED: 1` gives Speed II). Invalid effect names are ignored.                                                               |
| `role-limits`     | Section | The role limits for this upgrade. All the roles are in the format `ROLE-ID: LIMIT`, where the role id is the numeric id (weight) of the role from the main config, not its name.                                                                                |

## Price Types

The plugin has three pre-defined price types - money based prices, placeholders based prices and items based prices.\
When `price-type` is omitted, defaults to `money`. The value is case-insensitive. If an invalid price-type is used, the level will be skipped.\
Simply add the `price-type` section to your upgrade with the price-type you want. Currently there are three different ones: `money`, `placeholders` and `items`:

#### money

When using this price-type, money will be taken from the players' bank (Essentials or any other economy plugin).

You must add the following fields to your upgrade to get this working:

| Required Field | Type   | Description                        |
| -------------- | ------ | ---------------------------------- |
| `price`        | Double | The cost to upgrade to next level. |

#### placeholders

When using this price-type, money will be taken by executing custom commands, and the balance will be parsed by a placeholder.

You must add the following fields to your upgrade to get this working:

| Required Field      | Type   | Description                                                                                                                                        |
| ------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `price`             | Double | The cost to upgrade to next level.                                                                                                                 |
| `placeholder`       | String | The placeholder that represents the balance of the player.                                                                                         |
| `withdraw-commands` | List   | <p>A list of commands to be executed for withdrawing money.<br>You can use %player% for player's name and %amount% for the amount to withdraw.</p> |

#### items

When using this price-type, items will be taken from the players' inventory.

You must add the following fields to your upgrade to get this working:

| Required Field | Type    | Description                                                                                                                                                                                    |
| -------------  | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `amount`       | Integer | The amount of items to upgrade to next level.                                                                                                                                                  |
| `types`        | List    | <p>A list of item types, you can specify a single item or multiple items.<br>When multiple items are specified, the player can use any combination of them to meet the required amount.</p> |

{% hint style="info" %}
You can register custom price types using the API.
{% endhint %}

### Multiple prices

In the examples above, you saw that the `price-type` and other fields related to prices were placed directly within the upgrade level section. However, if you want to require multiple prices for a single upgrade level, you must create a `prices` section and place the prices inside it. You can find an example below.

### Example

In the example below, you can find upgrades with different price types.&#x20;

The first upgrade, `money-example-upgrade`, has a `money` price-type configured to it. The second one, `placeholders-example-upgrade`, has a `placeholders` price-type. The third upgrade, `items-example-upgrade`, has a `items` price-type configured to it. \
For this example, assume the placeholder `%custom_economy_balace%` returns an integer with the balance of the player and the command `/customeco take <player-name> <price>` takes the given balance from the given player.

```yaml
upgrades:
  money-example-upgrade:
    '1':
      price: 1000000.0
      price-type: 'money'
      commands:
        - ...
  placeholders-example-upgrade:
    '1':
      price: 1000000.0
      price-type: 'placeholders'
      placeholder: '%custom_economy_balance%'
      withdraw-commands:
      - 'customeco take %player% 1000000'
      commands:
        - ...
  items-example-upgrade:
    '1':
      amount: 64
      price-type: 'items'
      types:
      - 'DIAMOND'
      commands:
        - ...
  multiple-prices-example-upgrade:
    '1':
      prices:
        'money': # random, but unique key
          price: 1000000.0
          price-type: 'money'
        'customeco':
          price: 1000000.0
          price-type: 'placeholders'
          placeholder: '%custom_economy_balance%'
          withdraw-commands:
          - 'customeco take %player% 1000000'
        'diamonds':
          amount: 64
          price-type: 'items'
          types:
          - 'DIAMOND'
      commands:
        - ...
```
