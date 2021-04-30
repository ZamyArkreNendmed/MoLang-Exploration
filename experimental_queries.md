# Experimental MoLang Entity Queries Documentation [In-Progress] </h1>

* This beta documentation is made based on the version 1.17.0.52 of Minecraft: Bedrock Edition. Any changes may affect this documentation.
* This documentation is not yet completed.

### Creator Notes:
* You must enable `Holiday Creator Features` to enable the Experimental MoLang Features.
* Certain MoLang expressions, may not accept Entity Queries, causing the MoLang entity query to not parse.

| Experimental Entity Queries                                   |
|---------------------------------------------------------------|
| [query.get_nearby_entities](./experimental_queries.md#queryget_nearby_entities) |
| [query.get_nearby_entities_except_self](./experimental_queries.md#queryget_nearby_entities_except_self) |
| [query.scoreboard](./experimental_queries.md#queryscoreboard) |
| [query.self](./experimental_queries.md#queryself)             |

## Experimental Entity Queries



### query.get_nearby_entities
* This MoLang query returns the list of entities (Variable Reference) within the specified distance relative from the actor including the actor itself, taking an optional second argument as a filter for which mob or entity types to accept. For instance, 'minecraft:player', 'minecraft:zombie', 'minecraft:cat' and much more.
* You may need to use the `for_each` MoLang function to iterate the list. Refer an example below.
* You may also need to use `query.count` to get number of nearby entities.

| Argument Index | Type   | Default Value | Description                               |
|----------------|--------|---------------|-------------------------------------------|
| 0              | Number | 0.0           | The distance to test nearby entities relative from the actor |
| 1              | String |               | Entity identifier to filter from the list of nearby entities from the actor |

* Query writing example:<br>
`query.get_nearby_entities(<distance_in_metres>, '<entity_identifier>')`

* Return value:

| Type                  | Description                                                |
|-----------------------|------------------------------------------------------------|
| Entity Reference List | An array of Entities (Entity Reference) fetched which later used by `for_each` function to iterate the list |

<b> Situation </b><br>

a) For instance, you wanted to filter if there at least five entities in a ten block radius nearby an actor including itself, you would use `query.count` entity query and write this:<br>
  - `query.count(query.get_nearby_entities(10.0)) >= 5.0`<br>

b) For instance, you wanted to filter if there at least three cats in a nine block radius nearby an actor, you would use the following MoLang query `query.count` entity query and write this:<br>
  - `query.count(query.get_nearby_entities(9.0, 'minecraft:cats')) >= 3.0`<br>
  * As you can see above, it filters through the available entites including the current actor entity itself, and filter the entity identifier.

c) You also want to test if any nearby entities including itself within eight twelve block radius that are holding bow item and then select the entity:<br>
  - ```javascript
    variable.entity_holding_bow = 0.0;
    for_each(temp.entity, query.get_nearby_entities(12.0), {
      ((temp.entity -> query.get_equipped_item_name) == 'bow') ? {
        variable.entity_holding_bow = temp.entity;
        break;
      };
    });
    return variable.entity_holding_bow;
    ```
  * As you can see above, it will seach any nearby entities that are holding the specified item in their main hand, and save the selected entity for later use.
  * You can also notice that the `break` keyword will stop continuing the for each loop after it gets the selected entity.



### query.get_nearby_entities_except_self
* This MoLang query returns the list of entities (Variable Reference) within the specified distance relative from the actor with an exception of the actor itself, taking an optional second argument as a filter for which mob or entity types to accept. For instance, 'minecraft:cow', 'minecraft:sheep', 'minecraft:creeper' and much more.
* This MoLang query has similar functions to the MoLang query above, `query.get_nearby_entities` except that this query excludes the current actor entity.
* You may need to use the `for_each` MoLang function to iterate the list. Refer an example below.
* You may also need to use `query.count` to get number of nearby entities.

| Argument Index | Type   | Default Value | Description                               |
|----------------|--------|---------------|-------------------------------------------|
| 0              | Number | 0.0           | The distance to test nearby entities relative from the actor |
| 1              | String |               | Entity identifier to filter from the list of nearby entities from the actor |

* Query writing example:<br>
`query.get_nearby_entities_except_self(<distance_in_metres>, '<entity_identifier>')`

* Return value:

| Type                  | Description                                                |
|-----------------------|------------------------------------------------------------|
| Entity Reference List | An array of Entities (Entity Reference) fetched which later used by `for_each` function to iterate the list |

<b> Situation </b><br>

a) For instance, you wanted to filter if there at least five zombies in a nine block radius nearby an actor, you would use `query.count` entity query and write this:<br>
  - `query.count(query.get_nearby_entities_except_self(9.0, 'minecraft:zombie')) >= 5.0`<br>

b) You also want to test if any nearby entities within eight block radius that are sneaking and select the entity:<br>
  - ```javascript
    variable.entity_sneaking = 0.0;
    for_each(temp.entity, query.get_nearby_entities_except_self(8.0), {
      (temp.entity -> query.is_sneaking) ? {
        variable.entity_sneaking = temp.entity;
        break;
      };
    });
    return variable.entity_sneaking;
    ```
  * As you can see above, it will seach any nearby entities that are sneaking, and save the selected entity for later use.
  * You can also notice that the `break` keyword will stop continuing the for each loop after it gets the sneaking entity.



### query.scoreboard
* This MoLang entity query takes one argument which is the name of the scoreboard entry for an actor. It returns the specified scoreboard value for this entity.
* Note that this MoLang query only functions properly in Resource Pack when the scoreboard itself has a display (any).

| Argument Index | Type   | Default Value | Description                               |
|----------------|--------|---------------|-------------------------------------------|
| 0              | String |               | The scoreboard identifier of an actor     |

* Query writing example:<br>
`query.scoreboard('<scoreboard_name>')`

* Return value:

| Type   | Description                                                |
|--------|------------------------------------------------------------|
| Number | Any numerical value depending on the scoreboard identifier |


<b> Situation </b><br>

For instance, you have created a scoreboard with `counter` as the identifier, and you want to test it for an actor, you would write this:<br>
- `query.scoreboard('counter')`<br>

If you apply value `2` for the specified actor with the `counter` scoreboard, the returning value would be `2`. To test for if the `counter` scoreboard value is more than `2`:<br>
- `query.scoreboard('counter') > 2.0`<br>

This will return `1.0` or true if the scoreboard value is more than `2`.



### query.self
* This query returns the current entity (reference variable).

* Query writing example (with Arrow Operator):<br>
`query.self -> <Entity Reference>`

* Return value:

| Type             | Description                 |
|------------------|-----------------------------|
| Entity Reference | Returns an entity reference |


<b> Situation </b><br>

For instance, you want to query if this entity is sneaking, write:<br>
- `query.self -> query.is_sneaking`<br>

You can also refer 'structs' or variables on the entity referred.<br>
- `query.self -> variable.counter`
