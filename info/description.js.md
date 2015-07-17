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

- "id": (int) Unique identifier of the item. All items has this field.
- "player_id": (int) Ownership of the item.
- "role": (str) Describes the role of the item. It can be a `unit`, `tower`, `building`, `center`, or `obstacle`. You can read more below on the different roles.
- "hit_points": (int/float) Defines the durability of the item. If "hit_points" is zero or below, then
  the item is destroyed.
- "coordinates": (list of two int/float): Item location coordinates. Units are single point objects.
  For large objects such as buildings, this field contains the coordinates of the center (middle) point.
- "size": (int/float) Units don't have a size. All static objects (buildings, towers etc) are
  square and the edge length is equal to their "size".
- "action": (str) What the item is doing. It can be `idle`, `move`, `shoot`, or `charge`.
- "speed": (int/float) This is a unit attribute only. It describes how fast the unit may move.
- "damage_per_shot": (int/float) This is a unit/tower attribute which describes how many hit points a shot will take.
- "rate_of_fire": (int/float) This is a unit/tower attributewhich describes how many shots per second the item can do.
- "firing_range": (int/float) This is a unit/tower attributewhich describes the maximum distance it can shoot.


### Roles

You can use predefined constants instead of string variables.

```python
from battle import ROLE
```

- `unit` - Mobile fighting items. Appears from crafts. `ROLE.UNIT`
- `tower` - Stationary fighting items. `ROLE.TOWER`
- `center` - Command Centers. The main building. If it's destroyed, then a battle is over. `ROLE.CENTER`
- `building` - All other stationary buildings. `ROLE.BUILDING`
- `obstacle` - Neutral stationary objects such as rocks. `ROLE.OBSTACLE`

## Ask info

- `askMyInfo(callback)` Returns information about the current item.

- `askItemInfo(item_id, callback)` Returns information about the item with `id == item_id` or None.

- `askEnemyItems(callback)` Returns a list with information on the enemy items.

- `askMyItems(callback)` Returns a list with information on your items.

- `askBuildings(callback)` Returns a list with information for all buildings including the Command Center.

- `askTowers(callback)` Return a list with information of all towers.

- `askCenter(callback)` Returns information about the Command Center.

- `askUnits(callback)` Returns a list with information for all units.

- `askNearestEnemy(callback)` Returns information about the nearest enemy item.

- `askMyRangeEnemyItems()`  
    Returns a list with information on all enemies in the current items firing range.

## Commands.

- `doAttack(item_id, callback)` Attack the item with `id == item_id`.
    If the target is too far, then unit will move to the target.

- `doMove(coordinates, callback)` A unit only command.
    Move to the point with the given coordinates. _coordinates_: list/tuple of two int/float.


## Subscribes.

You can subscribe your units to an event, and when this event occurs the _callback_ function
will be called. The callback function will receive input data related to the subscription.
All subscriptions are disposable and removed when triggered.

- `whenInArea(center, radius, callback)` Triggered when the current unit is in
  the circle. _center_ are the coordinates of the center point and _radius_ is length of circle radius.

- `whenItemInArea(center, radius, callback)` The same as `subscribe_im_in_area` but
  triggered for any item.

- `whenIdle(callback)` Triggered when the current unit is idle (finishes moving or
  destroys an enemy or doesn't have commands).

- `whenEnemyInRange(callback)` Triggered when an enemy item is in the current item
  firing range.

- `whenEnemyOutRange(item_id, callback)` Triggered when the item with _item_id_ is
  out of the current item firing range.

- `whenItemDestroyed(item_id, callback)` Triggered when the item with _item_id_ is destroyed.

