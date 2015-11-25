Let's work on some new code for our defense buildings. 

The commanding principles are based on the 3 main groups of functions.

**Asks** are started with _ask_. Ask functions provide information about the unit you control or environment around it. For example, check out the following code...

```python
from battle import commander
tower_client = commander.Client()
my_info = tower_client.ask_my_info()
print("My ID:{}".format(my_info['id']))
```

... this shows a current towers's ID in the battle console.

**Actions** are started with _do_. The Action function sends a command to a current tower. The tower can only hold information about the last command, so every following command will overwrite previous one. For example, check out the following code...

```python
from battle import commander
tower_client = commander.Client()
tower_client.do_attack(1)
tower_client.do_attack(2)
```

... that code commands tower to attack unit with ID 2, but the tower will never attack unit with ID 1.

**Subscriptions** are started with _when_. The subscribe function always has a callback argument. Callback is the function that gets called when a specific event occurs. For example, check out the following code...

```python
from battle import commander
tower_client = commander.Client()

def unit_in_firing_range(data, **kwargs):
    tower_client.do_attack(data['id'])

tower_client.when_enemy_in_range(unit_in_firing_range)
```

... that commands the tower to attack any enemy that comes into its firing range.

**Prints**. Feel free to use the _print_ function and see every script's output in the right-hand panel for battle replays.


**You main goal is defend your command center**.

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

- `ask_my_info()` Returns information about the current item.

- `ask_item_info(item_id)` Returns information about the item with `id == item_id` or None.

- `ask_enemy_items()` Returns a list with information on the enemy items.

- `ask_my_items()` Returns a list with information on your items.

- `ask_buildings()` Returns a list with information for all buildings including the Command Center.

- `ask_towers()` Return a list with information of all towers.

- `ask_center()` Returns information about the Command Center.

- `ask_units()` Returns a list with information for all units.

- `ask_nearest_enemy()` Returns information about the nearest enemy item.

- `ask_my_range_enemy_items()`  
    Returns a list with information on all enemies in the current items firing range.

## Commands.

- `do_attack(item_id)` Attack the item with `id == item_id`.
    If the target is too far, then unit will move to the target.

 - `do_attack_coordinates(coordinates)` _(for RocketGun only)_ send a rocket to the point with the given coordinates. _coordinates_: list/tuple of two int/float.

## Subscribes.

You can subscribe your units to an event, and when this event occurs the _callback_ function
will be called. The callback function will receive input data related to the subscription.
All subscriptions are disposable and removed when triggered.

- `when_in_area(center, radius, callback)` Triggered when the current unit is in
  the circle. _center_ are the coordinates of the center point and _radius_ is length of circle radius.

- `when_item_in_area(center, radius, callback)` The same as `subscribe_im_in_area` but
  triggered for any item.

- `when_idle(callback)` Triggered when the current unit is idle (finishes moving or
  destroys an enemy or doesn't have commands).

- `when_enemy_in_range(callback)` Triggered when an enemy item is in the current item
  firing range.

- `when_enemy_out_range(item_id, callback)` Triggered when the item with _item_id_ is
  out of the current item firing range.

- `when_item_destroyed(item_id, callback)` Triggered when the item with _item_id_ is destroyed.
