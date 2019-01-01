# OpenHUD API

## HUD modules
A HUD module is an HTTPS endpoint that returns JSON and supports the following operations:

### Metadata
An HTTP GET request to the module's endpoint should return the module's metadata:

```javascript
{
    "title": "Your module's title",             // identifies your module in HUD tips
    "description": "Your module's description", // shown when configuring your module
    "games": [                                  // list of supported games
        "nlh"
    ],
    "author": {                                 // identifies module creator, email & URL are optional
        "name": "Your name",
        "email": "youremail@example.org",
        "url": "https://www.example.org/"
    }
}
```

### Tips
An HTTP POST request to the module's endpoint should return tips for a given situation.

POST body describes the situation:
```javascript
{
    "game": "nlh",                    // game type
    "bb": 2,                          // bb amount
    "community": ['As', 'Kd', Jc'],   // community cards (empty if none yet)
    "seats": [{                       // players by seat id, use isButton for positions
        "playerName": "playerName1",
        "stack": 48,
        "pot": 2,
        "isMe": true,
        "cards": ["Ad", "6s"]
    }, {
        "playerName": "playerName2",
        "stack": 98,
        "pot": 2
    }, {
        "playerName": "playerName3",
        "stack": 100,
        "pot": 0,
        "isButton": true,
        "isFolded": true
    }]
}

Above example describes the situation on the flop after the button folded pre-flop, hero called in SB and BB checked.

Response describes the tips to show next to each player (no tip shown for omitted players):
```javascript
{
    "players": {
        "playerName1": "Some tip",
        "playerName2": "Some other tip"
    }
}
```

### Errors
On any error, the endpoint should respond with an HTTP status code indicating error, and an error body in accordance with [RFC 7807](https://tools.ietf.org/html/rfc7807).
