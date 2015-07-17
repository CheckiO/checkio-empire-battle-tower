**Example:**

```javascript
var Client = require("battle/commander.js").Client;
var client = new Client();

function searchNextTarget() {
    client.askMyRangeEnemyItems(function (enemies) {
        if (enemies.length) {
            unitInFiringRange(enemies[0]);
        } else {
            client.whenEnemyInRange(unitInFiringRange);
        }
    });
}

function unitInFiringRange(data) {
    client.doAttack(data.id);
    client.whenIdle(searchNextTarget);
}

client.start(function () {
    searchNextTarget();
});

```
