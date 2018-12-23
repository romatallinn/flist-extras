# Flist Firebase Realtime Database

[![Flist](https://flist.me/css/favicons/android-icon-72x72.png)](https://flist.me)

Website: https://flist.me

### Overall
The database is constructed over the Google's Firebase Realtime Database. It is NoSQL.

It contains 6 branches:
 - **card-types**: describes all available default templates of the cards.
 - **cards**: contains the user's cards themselves.
 - **groups**: contains the user's groups that group cards together.
 - **following**: all followings the user has.
 - **followers**: all followers the user has.
 - **users**: the user's data set.

### Card Types

Structure:
```
<card_shortcut>
    -> icon:
    -> link:
    -> name:
```

Example:
```
fb
    -> icon: "fb.png"
    -> link: "https://facebook.com/"
    -> name: "Facebook"
```

**NB!** The link is supposed to be the one that leads to the user's profile and containing splash or other needed separator. So when the user writes his username in a dedicated field, it will be automatically attached to the end of the link.

### Cards
Structure:
```
<user_id>
    -> <card_id>
		-> name:
		-> username:
		-> description:
		-> group_id:
		-> privacy: (int)
		-> type:
		-> url:
```

Example: 
```
"NakM83PPXMfVvH0qqXpJRVOcVN83"
    -> "-LPD5ItN20sv62nOEVlX"
		-> name: "My Facebook"
		-> username: "Roma.Sirokov"
		-> description: "This is an example of the description."
		-> group_id: "-LPD4vI2oxXFveOFvzA6"
		-> privacy: "0"
		-> type: "fb"
		-> url: "https://twitter.com/SirokovR"
```
The 'type' field reflects the shortcuts of the default types from the 'card-type' table, if they are used, or 'none' if it is a custom card.

### Groups:
Structure:
```
<user_id>
    -> <group_id>
        -> name:
```

Example:
```
"NakM83PPXMfVvH0qqXpJRVOcVN83"
    -> "-LPD4vI2oxXFveOFvzA6"
        -> name: "Socials"
```

The 'group_id' is used in the 'cards' table if the card is in this group.

### Followers & Following

They are just reflections of each other. One shows all followers of a specific user, and the other shows all other users that a specific user is following.

Structure:
```
<user_id>
    -> <user_id>
        -> date: (DD/MM/YYYY)
```

Example:
```
"NakM83PPXMfVvH0qqXpJRVOcVN83"
    -> "6E4fBp8HIIbyjwbEysmpMFRoS5p1"
        -> date: "01/12/2018"
```

In the 'followers' table that would mean that the parent user is FOLLOWED by a child user, but in the 'following' table that would mean that the parent user is FOLLOWING a child user.

**NB!** The mirroring happens automatically by the Firebase Cloud Functions. You can find the scripts in the special [repository](https://github.com/romatallinn/flist-firebase-funcs).

### Users
Structure:
```
<user_id>
    -> name:
    -> surname:
    -> username:
    -> username_lowercased:
    -> description:
    -> img:
    -> img_update: (DD/MM/YYYY)
    -> verified: (bool)
    -> fcm_tokens
        -> <token_id>: true
        -> ...
```

Example:
```
"NakM83PPXMfVvH0qqXpJRVOcVN83"
    -> name: "Roman"
    -> surname: "Sirokov"
    -> username: "RSirokov"
    -> username_lowercased: "rsirokov"
    -> description: "Studying BSc CS&E at TU Delft"
    -> img: "NakM83PPXMfVvH0qqXpJRVOcVN83.png"
    -> img_update: "11/11/2018"
    -> verified: true
    -> fcm_tokens
        -> "c-7wEoe3Tsw:APA91bFtY9HzhhJ2qvIQXsq_nYmcjl0ieGRlONda": true
        -> ...
```

### Other
The rules that are used in regulating the security of the database access are in the respective file.

