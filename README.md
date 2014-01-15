hive-frame-members
==================

Adds social membership based on passport. Members log in through oauth via facebook or twitter and are tracked
via mongo records.

This requires a Twitter and/or Facebook application tied to your domain.
For local development, edit your /etc/hosts file to simulate transactions from / to your domain.

 Membership includes ACL/permission, which is defined in frame config files through role and action nodes:

 ``` json


	"member_actions": [
        "foo create",
		"foo edit",
		"foo delete"
	],

	"member_roles": [
		{
			"name":    "foo user",
			"actions": [
                "foo create"
			]

		},
		{
			"name":    "foo editor",
			"actions": [
                "foo edit",
                "foo delete"
			]

		}

	]

```

The Members module requires several parameters to be defined in the apiary; the easiest way to do this is to define
a file "passport_config.json" in the root with the parameters defined like the examples below.

``` json
{
    "facebook_app_id": "app_id",
    "facebook_app_secret": "secret",

    "twitter_consumer_key": 	"key",
    "twitter_consumer_secret": "secret",

    "twitter_access_token":	"token",
    "twitter_access_token_secret":	"secret"
}
```

... then include this line

``` js
    mongoose.connect('mongodb://localhost/my_db');
    var apiary = mvc.Apiary({mongoose: mongoose}, frame_path);
    // ...
    apiary._config.setAll(require('./passport_config.json'));
    // ...
    apiary.init(function () { // ...
```