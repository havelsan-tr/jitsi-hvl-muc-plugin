# Jitsi Hvl-Muc Plugin
Jitsi Hvl-Muc Plugin is a plugin that provides you local APIs to manage and get your jitsi data. API endpoints can only be accessed from localhost. **There is no authentication protection, that's why don't make it accessible from remote.**


# Installation

Copy `mod_hvl_muc.lua` and `mod_hvl_api.lua` to

`/usr/share/jitsi-meet/prosody-plugins/`

Install dependency

`sudo apt install lua-sql-sqlite3`

Enable module `hvl_api` from `/etc/prosody/conf.d/localhost.cfg.lua` by adding this

```
VirtualHost "localhost"
    app_id=""
    app_secret=""
    authentication = "anonymous"
    modules_enabled = {
        "hvl_api";
    }
```

Then enable `hvl_muc` module from `/etc/prosody/conf.d/$HOSTNAME.cfg.lua` by changing this

```
Component "conference.$HOSTNAME" "muc"
    storage = "memory"
    modules_enabled = {
        "muc_meeting_id";
        "muc_domain_mapper";
        "hvl_muc"; <---- ADD THIS LINE
        -- "token_verification";
    }
    admins = { "focus@auth.$HOSTNAME" }
```

# Doc

# Get Room Activity Stats
**URL** : `/room_stats?page={PAGE_NUMBER=1}`

**Method** : `GET`

**cURL** : `curl "http://localhost:5280/room_stats?page={PAGE_NUMBER=1}"`

## Success Responses

**Code** : `200 OK`

**Content** :
```json
{
    "total_page": 1,
    "page_size": 25,
    "current_page": 1,
    "data": [
        {
            "created_at": "1585850900000",
            "jid": "deneme@conference.jitsi.baran.lab",
            "occupants": [
                {
                    "created_at": "1585850901000",
                    "jid": "deneme@conference.jitsi.baran.lab/b06df7aa",
                    "room_jid": "deneme@conference.jitsi.baran.lab",
                    "email": "baransekin@gmail.com",
                    "display_name": "baransekin"
                },
                {
                    "created_at": "1585850922000",
                    "jid": "deneme@conference.jitsi.baran.lab/2e639f6c",
                    "room_jid": "deneme@conference.jitsi.baran.lab",
                    "email": "",
                    "display_name": "deneme"
                }
            ],
            "name": "deneme",
            "password": ""
        }
    ]
}
```

# Get Active Rooms
**URL** : `/rooms`

**Method** : `GET`

**cURL** : `curl "http://localhost:5280/rooms"`

## Success Responses

**Code** : `200 OK`

**Content** :
```json
[
    {
        "name": "jvbbrewery",
        "jid": "jvbbrewery@internal.auth.jitsi.baran.lab"
    },
    {
        "name": "deneme",
        "jid": "deneme@conference.jitsi.baran.lab"
    }
]
```

# Get Specific Room Detail

**URL** : `http://localhost:5280/room?room={ROOM_NAME}`

**Method** : `GET`

**cURL** : `curl "http://localhost:5280/room?room={ROOM_NAME}"`

## Success Responses

**Code** : `200 OK`

**Content** :
```json
{
    "room": {
        "jid": "deneme@conference.jitsi.baran.lab",
        "password": "",
        "name": "deneme",
        "conference_duration": 1585752911000
    },
    "occupants": [
        {
            "email": "baransekin@gmail.com",
            "jid": "deneme@conference.jitsi.baran.lab/3c7a5c85",
            "display_name": "baransekin"
        }
    ]
}
```

# Create Room

**URL** : `http://localhost:5280/room?room={ROOM_NAME}`

**Method** : `PUT`

**cURL** : `curl -X PUT "http://localhost:5280/room?room={ROOM_NAME}"`

## Success Responses

**Code** : `200 OK`

# Destroy Room

**URL** : `http://localhost:5280/room?room={ROOM_NAME}`

**Method** : `DELETE`

**cURL** : `curl -X DELETE "http://localhost:5280/room?room={ROOM_NAME}"`

## Success Responses

**Code** : `200 OK`

# Change Room Password

**URL** : `http://localhost:5280/room?room={ROOM_NAME}&password={PASSWORD}`

**Method** : `PATCH`

**cURL** : `curl -X PATCH "http://localhost:5280/room?room={ROOM_NAME}&password={PASSWORD}"`

## Success Responses

**Code** : `200 OK`