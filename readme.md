# RoleSpell Pre-Alpha [![Stories in Ready](https://badge.waffle.io/MikroAcse/RoleSpell.svg?label=ready&title=Ready)](http://waffle.io/MikroAcse/RoleSpell)

RoleSpell is simple 2D RPG game made in Java with LibGDX.

![](https://raw.githubusercontent.com/MikroAcse/RoleSpell/master/screenshots/screencap.gif)

## Implemented features
  - TMX map loading/parsing;
  - Asset and config loading/parsing via custom AssetManager;
  - Item and entity config parsers;
  - Entity behaviors, such as: Attack, Pickup, Flee, Seek, Wander and Teleport behaviors;
    They are controlled by custom BehaviorAi component;
  - Inventory, movement and status components;
  - A* for path finding (entities avoid obstacles and prefer to walk along the paths);
  - Teleportation between locations;
  - Map, entities and HUD rendering (including rendering of inventory, hotbar, entity names, current entity's path and status properties);
  - Dropping/picking up items, opening and closing inventory HUD, adding items to the hotbar;
  - And many others.

## TODO (Alpha)
  - Shared configs (i.e. avoid duplication of player's config in map configs);
  - World manager (do not recreate world on teleporting);
  - Highlight targeted entities;
  - Fight animation (i.e. "swipe" for sword);
  - Tooltips for inventory;
  - Experience and stamina properties;
  - Better status HUD.

#### [View full TODO list on Waffle.io](https://waffle.io/MikroAcse/RoleSpell)

## Config examples

Asset list for game bundle (resources/game/config.json):
```json
{
  "files": {
    "textures": [
      "entities/*",
      "inventory/*",
      "items/*",
      "hud/*",
      "ui/*"
    ],
    "maps" : [
      "rottenville/map.tmx",
      "eclipse-chambers/map.tmx"
    ],
    "configs": [
      "items.json",
      "maps/*"
    ]
  }
}
```

Map config (resources/game/configs/maps/*.json):
```json
{
  "name": "locations.eclipse_chambers.name",

  "spawners": {
    "player": {
      "type": "ONCE",
      "entity-type": "PLAYER",
      "shared": true,

      "inventory": [
        {
          "item": "hand",
          "hotbar": 0
        },
        {
          "item": "light-orb"
        },
        {
          "item": "beginner-staff",
          "index": 5,
          "hotbar": 2
        },
        {
          "item": {
            "parent": "ultimate-sword",
            "name": "ULTRA SWORD!11"
          },
          "index": 10,
          "hotbar": 1,
          "chance": 0.5
        }
      ]
    },

    "monster-spawner": {
      "type": "SPAWNER",
      "entity-type": "MONSTER",
      "interval": 2,
      "radius": 1,
      "checker": {
        "type": "MONSTER_SPAWNER",
        "radius": "5"
      }
    },

    "ogremagi": {
      "type": "ONCE",
      "entity-type": "OGREMAGI",
      "name": "Ogremagi",

      "inventory": [
        {
          "item": "ultimate-sword",
          "chance": 0.5
        },
        {
          "item": "wooden-sword"
        }
      ]
    },

    "elizabeth": {
      "type": "ONCE",
      "entity-type": "NPC",
      "name": "Elizabeth"
    },

    "dropped-sword": {
      "type": "ONCE",
      "entity-type": "DROPPED_ITEM",
      "item": "ultimate-sword"
    }
  },

  "portals": {
    "rottenville": {
      "destination": "rottenville",
      "portal-id": "rottenville-eclipse-chambers",
      "name": "locations.rottenville.name"
    }
  },

  "meta": {
    "EMPTY": {
      "weight": 1.0,
      "passable": true
    },
    "SOLID": {
      "weight": 0.0,
      "passable": false
    },
    "WATER": {
      "weight": 0.0,
      "passable": false
    },
    "PATH": {
      "weight": 0.9,
      "passable": true
    }
  }
}
```

Items config:
```json
{
  "abstract-item": {
    "throwable": false,
    "pickable": false
  },

  // ---------------------------------------- WEAPONS

  "abstract-weapon": {
    "parent": "abstract-item",
    "type": "WEAPON",

    "throwable": true,
    "pickable": true,

    "parameters": {
      "attack-timer": { "min": 1.0, "max": 3.0 },
      "limit": 0,
      "attack-distance": 1,
      "damage": 0
    }
  },

  "hand": {
    "parent": "abstract-weapon",
    "texture": "items/weapons/hand",
    "name": "items.weapons.hand",

    "throwable": false,
    "pickable": false,

    "parameters": {
      "weapon-type": "HAND",

      "attack-timer": 1.5,
      "damage": 5
    }
  },

  // --------------- SWORDS

  "abstract-sword": {
    "parent": "abstract-weapon",

    "parameters": {
      "weapon-type": "WEAPON_SWORD"
    }
  },

  "wooden-sword": {
    "parent": "abstract-sword",
    "name": "items.weapons.wooden_sword",
    "texture": "items/weapons/wooden-sword",

    "parameters": {
      "damage": { "min": 5.0, "max": 15.0 },
      "attack-distance": 1
    }
  },

  "ultimate-sword": {
    "parent": "abstract-sword",
    "name": "items.weapons.ultimate_sword",
    "texture": "items/weapons/ultimate-sword",

    "parameters": {
      "damage": 25,
      "attack-distance": 1
    }
  },

  // --------------- STAVES

  "abstract-staff": {
    "parent": "abstract-weapon",

    "parameters": {
      "weapon-type": "STAFF"
    }
  },

  "beginner-staff": {
    "parent": "abstract-staff",
    "name": "items.weapons.beginner_staff",
    "texture": "items/weapons/beginner-staff",

    "parameters": {
      "damage": { "min":5, "max":10 },
      "attack-distance": 5
    }
  },

  "epic-staff": {
    "parent": "abstract-staff",
    "name": "items.weapons.epic_staff",
    "texture": "items/weapons/epic-staff",

    "parameters": {
      "damage": 10,
      "attack-distance": 10
    }
  },

  // --------------- CUSTOM

  "light-orb": {
    "parent": "abstract-weapon",
    "name": "items.weapons.light_orb",
    "texture": "items/weapons/light-orb",

    "parameters": {
      "weapon-type": "ORB",
      "attack-distance": 20,
      "damage": 20,
      "limit": 1
    }
  }
}
```
