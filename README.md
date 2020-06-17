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
    onGameSettings: function() {},
    onGameStart: function() {},
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
