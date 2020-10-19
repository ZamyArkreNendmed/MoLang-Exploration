# Experimental MoLang Entity Queries Documentation </h1>

* This beta documentation is made based on the version 1.16.100.60 of Minecraft: Bedrock Edition.

| Experimental Entity Queries                                   |
|---------------------------------------------------------------|
| [query.scoreboard](./experimental_queries.md#queryscoreboard) |
| [query.self](./experimental_queries.md#queryself)             |

## Experimental Entity Queries



### query.scoreboard
* Takes one argument which is the name of the scoreboard entry for an actor. Returns the specified scoreboard value for this entity.

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
* Returns the current entity (reference variable).

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
