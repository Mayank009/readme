## MX Player Games Documentation

MX Player - HTML5 Games Integration

### Integration Steps:

The MXPlayer App opens the game in a web-view. These games are opened by the App in 2 ways:

* Uploading a zip file
* Directly by a url.

In a zip file based approach, the game is downloaded only when the user clicks on a particular game inside the `Games` tab in the app. Also whenever a new version of a game is uploaded, the app re-downloads the game to get the latest build.

A Script has to be included in the game initialisation which basically adds an object `cc` in the window scope. This helps in communication from app to game. Please contact us to get the script.

When the web-view opens a game, it passes an object `gameManager` to the window scope which serves the communication from the game to the app. The object format is:

```
window.gameManager = {
    onGameInit: function() {},
    onGameStart: function() {},
    onGameSettings: function() {},
    onTrack: function() {},
    onError: function() {},
    onCheckRewardedVideoAds: function() {},
    onShowRewardedVideoAds: function() {},
    onGameOver: function() {}
}
```

Let’s go through each of these methods:

### gameManager.onGameInit()

This function returns few common parameters used across all games like:
roomId, userId, gameId, highestScore, gameMode, isFirstOpen

```
try {
    if (typeof gameManager !== 'undefined') {
        const gameConfigString = gameManager.onGameInit()
        const config = JSON.parse(gameConfigString)
        const {userId, gameId, roomId, highestScore, gameMode, isFirstOpen} = config
    }
} catch (e) {
    console.log("Error Parsing Config")
}
```

Note: The data received is a stringified JSON

Currently the returned object has the following fields:

```
var config = {
   userId: "1",
   gameId: "2",
   roomId: "3",
   highestScore: 90,
   lastLevel: 0,
   gameMode: "score", // "score" or "win"
   isFirstOpen: true, // true or false
   roomType: "free_room" // "free_room" or "priced_room"
}
```

### gameManager.onGameStart()

This function has to be called by the web-view when the game is ready and set to play, to notify the App that the game is ready.

```
if (typeof gameManager !== 'undefined') {
    try {
        gameManager.onGameStart()
    } catch (e) {
        gameManager.onError(e.stack.toString())
    }
}
```

### gameManager.getGameSettings()

This function is called to get configurable parameters for the game. Many difficulty, powerup, probability etc can be configured to be received through this function. MX Player CMS takes these inputs and passes them onto the game through this function. This way, no code changes are required in the game parameters are to be altered.

```
var defaultConfig = {
    reviveScore: 0,
    reviveEnabled: true,
    reviveLives: 1,
    reviveAdExistsDefault: true,
    autoAd: true,
    noDieScore: 10
}

var loadConfig = function () {
    if (typeof gameManager !== 'undefined') {
        try {
            var gameSettingString = gameManager.getGameSettings()
            var config = JSON.parse(gameSettingString)
            return config
        } catch (e) {
            return defaultConfig
        }
    } else {
        return defaultConfig
    }
}

var config = loadConfig()

module.exports = config
```

Note: The data received is a stringified JSON
