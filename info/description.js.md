Here is where we will code our tower.

This code runs when a battle start.    
The tower can ask for information about a battle or subscribe to various events.

**You main goal is defend your command center**.

## Get Started

SORRY, TBD

## Items

Units, towers, buildings and other objects on a map are called "items".
When you ask for info about items, you will receive a dictionary with the item data, or
a list of these dictionaries. The item info for can contain various fields.
Thus it is better to use the `dict.get` method. An item can have the following keys:

- "id": (int) Unique identifier for the item. All items have this field.
- "player_id": (int) the ownership of the item.
- "role": (str) Describes the role of the item. It can be a `unit`, `tower`, `building`, `center`, or `obstacle`. You can read more below on the different roles.
- "type": (str) Describes the type of the item. It can be a `sentryGun`, `infantryBot` etc.
- "hit_points": (int/float) Defines the durability of the item. If "hit_points" is zero or lower, the item is destroyed.
- "coordinates": (list of two int/float): The item's location coordinates. Units are single point objects.
  For large objects such as buildings, this field contains the coordinates of the center (middle) point.
- "size": (int/float) Units don't have a size. All static objects (buildings, towers etc) are square and the edge length is equal to their "size".
- "status": (dict) What the item is doing.
- "speed": (int/float) This is a unit attribute only. It describes how fast the unit may move.
- "damage_per_shot": (int/float) This is a unit/tower attribute which describes how many hit points an attack will take.
- "rate_of_fire": (int/float) This is a unit/tower attribute which describes how many attacks per second the item can take.
- "firing_range": (int/float) This is a unit/tower attribute which describes the maximum distance it can attack.


### Roles

You can use predefined constants instead of string variables.

```javascript
var ROLE = require("battle/terms.js").ROLE;
```

- `unit` - Mobile fighting items. Appears from crafts. `ROLE.UNIT`
- `tower` - Stationary fighting items. `ROLE.TOWER`
- `center` - Command Centers. The main building. If it's destroyed, then a battle is over. `ROLE.CENTER`
- `building` - All other stationary buildings. `ROLE.BUILDING`
- `obstacle` - Neutral stationary objects such as rocks. `ROLE.OBSTACLE`

## Ask info

- `askMyInfo()` Returns information about the current item.

- `askItemInfo(item_id)` Returns information about the item with `id == item_id` or None.

- `askEnemyItems()` Returns a list with information on the enemy items.

- `askMyItems()` Returns a list with information on your items.

- `askBuildings()` Returns a list with information for all buildings including the Command Center.

- `askTowers()` Returns a list with information for all towers.

- `askCenter()` Returns information about the Command Center.

- `askUnits()` Returns a list with information for all units.

- `askNearestEnemy()` Returns a list with information on all enemies in the current item's firing range.

- `askMyRangeEnemyItems()`  
    Returns a list with information on all enemies in the current items firing range.

```javascript
var ROLE = require("battle/terms.js").ROLE;
near_tower = client.askNearestEnemy([ROLE.TOWER])
```

- `askCurTime()`
    Returns current in-game time. (secs)


## Commands.

- `doAttack(item_id)` Attack the item with `id == item_id`.
    If the target is too far, then unit will move to the target.

### LEVEL 4

for units with level 4 or more.

- `doMessageToId(message, item_id)` send a message to a unit with `item_id`.

- `doMessageToCraft(message)` send a message to all units from your craft.

- `doMessageToTeam(message)` send a message to all units from your team.


## Subscribes.

You can subscribe your units to an event, and when this event occurs the _callback_ function
will be called. The callback function will receive input data related to the subscription.
All subscriptions are disposable and removed when triggered.

_every function in this category returns a promice object so you can pass your callback function into its then method. For example:_

```javascript
client.whenItemDestroyed(data.id).then(attackNearest);
```

_Here https://www.promisejs.org/ you can find more about promices functionality_

- `whenInArea(center, radius)` Triggered when the current unit is in
  the circle. _center_ are the coordinates of the center point and _radius_ is length of circle radius.

- `whenItemInArea(center, radius)` The same as `subscribe_im_in_area` but
  triggered for any item.

- `whenIdle()` Triggered when the current unit is idle (finishes moving or
  destroys an enemy or doesn't have commands).

- `whenEnemyInRange()` Triggered when an enemy item is in the current item
  firing range.

- `whenEnemyOutRange(item_id)` Triggered when the item with _item_id_ is
  out of the current item firing range.

- `whenItemDestroyed(item_id)` Triggered when the item with _item_id_ is destroyed.

### LEVEL 2

for units with level 2 or more.

- `whenTime(secs)` Is triggered at a specific game time. Very useful for the synchronization of units.

### LEVEL 4

for units with level 4 or more.

- `whenMessage()` Is triggered when a unit gets a message from another unit. Be aware that promise only resolve the first message in the stack. If you want to get another, you should call whenMessage again.

