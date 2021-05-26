<p align="center">
  <img src="/kafka-connect.png" alt="kafka-connect-diagram"/>
</p>
Just an experimental project using the [Twitter Source connector](https://github.com/jcustenborder/kafka-connect-twitter) to produce tweets as messages.

## Getting started

Fill out the values in `twitter.properties.example` with your Twitter API keys/values
```bash
name=TwitterSourceConnector
tasks.max=1
connector.class=com.github.jcustenborder.kafka.connect.twitter.TwitterSourceConnector

# Set these required values
twitter.oauth.accessTokenSecret=
process.deletes=
filter.keywords=
kafka.status.topic=
kafka.delete.topic=
twitter.oauth.consumerSecret=
twitter.oauth.accessToken=
twitter.oauth.consumerKey=
```

Move the file over

```bash
❯ mv twitter.properies.example twitter.properies
```

Start the kafka server & zookeeper in (separate windows)

```bash
# Kafka server
❯ kafka-server-start config/server.properties

# Zookeeper
❯ zookeeper-server-start.sh config/zookeeper.properties
```

Create status & delete topics

```bash
# Status topic
❯ kafka-topics --zookeeper localhost:2181 --create --topic <your-topic-status-name> --partitions 3 --replication-factor 1
Created topic <your-topic-status-name>.

# Delete topic
❯ kafka-topics --zookeeper localhost:2181 --create --topic <your-topic-delete-name> --partitions 3 --replication-factor 1
Created topic <your-topic-delete-name>.
```

Optional: Check your topic was created with the right partitions & replication factor

```bash
❯ kafka-topics --zookeeper localhost:2181 --topic <your-topic-name> --describe
Topic: <your-topic-name>	TopicId: KfCandFCTbCaGEwraEPq3Q	PartitionCount: 3	ReplicationFactor: 1	Configs:
	Topic: <your-topic-name>	Partition: 0	Leader: 0	Replicas: 0	Isr: 0
	Topic: <your-topic-name>	Partition: 1	Leader: 0	Replicas: 0	Isr: 0
	Topic: <your-topic-name>	Partition: 2	Leader: 0	Replicas: 0	Isr: 0
```

Run both consumers in separate terminals:

```bash
# Status topic
❯ kafka-console-consumer --bootstrap-server localhost:9092 --topic <your-topic-status-name>

# Delete topic
❯ kafka-console-consumer --bootstrap-server localhost:9092 --topic <your-topic-delete-name>
```

Run the executable

```bash
sh execute.sh
```

Observe the consumer logs for status & delete

```bash
{"schema":{"type":"struct","fields":[{"type":"int64","optional":true,"name":"org.apache.kafka.connect.data.Timestamp","version":1,"doc":"Return the created_at","field":"CreatedAt"},{"type":"int64","optional":true,"doc":"Returns the id of the status","field":"Id"},{"type":"string","optional":true,"doc":"Returns the text of the status","field":"Text"},{"type":"string","optional":true,"doc":"Returns the source","field":"Source"},{"type":"boolean","optional":true,"doc":"Test if the status is truncated","field":"Truncated"},{"type":"int64","optional":true,"doc":"Returns the in_reply_tostatus_id","field":"InReplyToStatusId"},{"type":"int64","optional":true,"doc":"Returns the in_reply_user_id","field":"InReplyToUserId"},{"type":"string","optional":true,"doc":"Returns the in_reply_to_screen_name","field":"InReplyToScreenName"},{"type":"struct","fields":[{"type":"double","optional":false,"doc":"returns the latitude of the geo location","field":"Latitude"},{"type":"double","optional":false,"doc":"returns the longitude of the geo location","field":"Longitude"}],"optional":true,"name":"com.github.jcustenborder.kafka.connect.twitter.GeoLocation","doc":"Returns The location that this tweet refers to if available.","field":"GeoLocation"},{"type":"struct","fields":[{"type":"string","optional":true,"field":"Name"},{"type":"string","optional":true,"field":"StreetAddress"},{"type":"string","optional":true,"field":"CountryCode"},{"type":"string","optional":true,"field":"Id"},{"type":"string","optional":true,"field":"Country"},{"type":"string","optional":true,"field":"PlaceType"},{"type":"string","optional":true,"field":"URL"},{"type":"string","optional":true,"field":"FullName"}],"optional":true,"name":"com.github.jcustenborder.kafka.connect.twitter.Place","doc":"Returns the place attached to this status","field":"Place"},{"type":"boolean","optional":true,"doc":"Test if the status is favorited","field":"Favorited"},{"type":"boolean","optional":true,"doc":"Test if the status is retweeted","field":"Retweeted"},{"type":"int32","optional":true,"doc":"Indicates approximately how many times this Tweet has been \"favorited\" by Twitter users.","field":"FavoriteCount"},{"type":"struct","fields":[{"type":"int64","optional":true,"doc":"Returns the id of the user","field":"Id"},{"type":"string","optional":true,"doc":"Returns the name of the user","field":"Name"},{"type":"string","optional":true,"doc":"Returns the screen name of the user","field":"ScreenName"},{"type":"string","optional":true,"doc":"Returns the location of the user","field":"Location"},{"type":"string","optional":true,"doc":"Returns the description of the user","field":"Description"},{"type":"boolean","optional":true,"doc":"Tests if the user is enabling contributors","field":"ContributorsEnabled"},{"type":"string","optional":true,"doc":"Returns the profile image url of the user","field":"ProfileImageURL"},{"type":"string","optional":true,"field":"BiggerProfileImageURL"},{"type":"string","optional":true,"field":"MiniProfileImageURL"},{"type":"string","optional":true,"field":"OriginalProfileImageURL"},{"type":"string","optional":true,"field":"ProfileImageURLHttps"},{"type":"string","optional":true,"field":"BiggerProfileImageURLHttps"},{"type":"string","optional":true,"field":"MiniProfileImageURLHttps"},{"type":"string","optional":true,"field":"OriginalProfileImageURLHttps"},{"type":"boolean","optional":true,"doc":"Tests if the user has not uploaded their own avatar","field":"DefaultProfileImage"},{"type":"string","optional":true,"doc":"Returns the url of the user","field":"URL"},{"type":"boolean","optional":true,"doc":"Test if the user status is protected","field":"Protected"},{"type":"int32","optional":true,"doc":"Returns the number of followers","field":"FollowersCount"},{"type":"string","optional":true,"field":"ProfileBackgroundColor"},{"type":"string","optional":true,"field":"ProfileTextColor"},{"type":"string","optional":true,"field":"ProfileLinkColor"},{"type":"string","optional":true,"field":"ProfileSidebarFillColor"},{"type":"string","optional":true,"field":"ProfileSidebarBorderColor"},{"type":"boolean","optional":true,"field":"ProfileUseBackgroundImage"},{"type":"boolean","optional":true,"doc":"Tests if the user has not altered the theme or background","field":"DefaultProfile"},{"type":"boolean","optional":true,"field":"ShowAllInlineMedia"},{"type":"int32","optional":true,"doc":"Returns the number of users the user follows (AKA \"followings\")","field":"FriendsCount"},{"type":"int64","optional":true,"name":"org.apache.kafka.connect.data.Timestamp","version":1,"field":"CreatedAt"},{"type":"int32","optional":true,"field":"FavouritesCount"},{"type":"int32","optional":true,"field":"UtcOffset"},{"type":"string","optional":true,"field":"TimeZone"},{"type":"string","optional":true,"field":"ProfileBackgroundImageURL"},{"type":"string","optional":true,"field":"ProfileBackgroundImageUrlHttps"},{"type":"string","optional":true,"field":"ProfileBannerURL"},{"type":"string","optional":true,"field":"ProfileBannerRetinaURL"},{"type":"string","optional":true,"field":"ProfileBannerIPadURL"},{"type":"string","optional":true,"field":"ProfileBannerIPadRetinaURL"},{"type":"string","optional":true,"field":"ProfileBannerMobileURL"},{"type":"string","optional":true,"field":"ProfileBannerMobileRetinaURL"},{"type":"boolean","optional":true,"field":"ProfileBackgroundTiled"},{"type":"string","optional":true,"doc":"Returns the preferred language of the user","field":"Lang"},{"type":"int32","optional":true,"field":"StatusesCount"},{"type":"boolean","optional":true,"field":"GeoEnabled"},{"type":"boolean","optional":true,"field":"Verified"},{"type":"boolean","optional":true,"field":"Translator"},{"type":"int32","optional":true,"doc":"Returns the number of public lists the user is listed on, or -1 if the count is unavailable.","field":"ListedCount"},{"type":"boolean","optional":true,"doc":"Returns true if the authenticating user has requested to follow this user, otherwise false.","field":"FollowRequestSent"},{"type":"array","items":{"type":"string","optional":false},"optional":false,"doc":"Returns the list of country codes where the user is withheld","field":"WithheldInCountries"}],"optional":false,"name":"com.github.jcustenborder.kafka.connect.twitter.User","doc":"Return the user associated with the status. This can be null if the instance is from User.getStatus().","field":"User"},{"type":"boolean","optional":true,"field":"Retweet"},{"type":"array","items":{"type":"int64","optional":false},"optional":false,"doc":"Returns an array of contributors, or null if no contributor is associated with this status.","field":"Contributors"},{"type":"int32","optional":true,"doc":"Returns the number of times this tweet has been retweeted, or -1 when the tweet was created before this feature was enabled.","field":"RetweetCount"},{"type":"boolean","optional":true,"field":"RetweetedByMe"},{"type":"int64","optional":true,"doc":"Returns the authenticating user's retweet's id of this tweet, or -1L when the tweet was created before this feature was enabled.","field":"CurrentUserRetweetId"},{"type":"boolean","optional":true,"field":"PossiblySensitive"},{"type":"string","optional":true,"doc":"Returns the lang of the status text if available.","field":"Lang"},{"type":"array","items":{"type":"string","optional":false},"optional":false,"doc":"Returns the list of country codes where the tweet is withheld","field":"WithheldInCountries"},{"type":"array","items":{"type":"struct","fields":[{"type":"string","optional":true,"doc":"Returns the text of the hashtag without #.","field":"Text"},{"type":"int32","optional":true,"doc":"Returns the index of the start character of the hashtag.","field":"Start"},{"type":"int32","optional":true,"doc":"Returns the index of the end character of the hashtag.","field":"End"}],"optional":false,"name":"com.github.jcustenborder.kafka.connect.twitter.HashtagEntity","doc":""},"optional":true,"doc":"Returns an array if hashtag mentioned in the tweet.","field":"HashtagEntities"},{"type":"array","items":{"type":"struct","fields":[{"type":"string","optional":true,"doc":"Returns the name mentioned in the status.","field":"Name"},{"type":"int64","optional":true,"doc":"Returns the user id mentioned in the status.","field":"Id"},{"type":"string","optional":true,"doc":"Returns the screen name mentioned in the status.","field":"Text"},{"type":"string","optional":true,"doc":"Returns the screen name mentioned in the status.","field":"ScreenName"},{"type":"int32","optional":true,"doc":"Returns the index of the start character of the user mention.","field":"Start"},{"type":"int32","optional":true,"doc":"Returns the index of the end character of the user mention.","field":"End"}],"optional":false,"name":"com.github.jcustenborder.kafka.connect.twitter.UserMentionEntity","doc":""},"optional":true,"doc":"Returns an array of user mentions in the tweet.","field":"UserMentionEntities"},{"type":"array","items":{"type":"struct","fields":[{"type":"int64","optional":true,"doc":"Returns the id of the media.","field":"Id"},{"type":"string","optional":true,"doc":"Returns the media type photo, video, animated_gif.","field":"Type"},{"type":"string","optional":true,"doc":"Returns the media URL.","field":"MediaURL"},{"type":"map","keys":{"type":"int32","optional":false},"values":{"type":"struct","fields":[{"type":"int32","optional":true,"doc":"","field":"Resize"},{"type":"int32","optional":true,"doc":"","field":"Width"},{"type":"int32","optional":true,"doc":"","field":"Height"}],"optional":false,"name":"com.github.jcustenborder.kafka.connect.twitter.MediaEntity.Size","doc":""},"optional":false,"field":"Sizes"},{"type":"string","optional":true,"doc":"Returns the media secure URL.","field":"MediaURLHttps"},{"type":"int32","optional":true,"doc":"","field":"VideoAspectRatioWidth"},{"type":"int32","optional":true,"doc":"","field":"VideoAspectRatioHeight"},{"type":"int64","optional":true,"doc":"","field":"VideoDurationMillis"},{"type":"array","items":{"type":"struct","fields":[{"type":"string","optional":true,"doc":"","field":"Url"},{"type":"int32","optional":true,"doc":"","field":"Bitrate"},{"type":"string","optional":true,"doc":"","field":"ContentType"}],"optional":false,"name":"com.github.jcustenborder.kafka.connect.twitter.ExtendedMediaEntity.Variant","doc":""},"optional":true,"doc":"Returns size variations of the media.","field":"VideoVariants"},{"type":"string","optional":true,"doc":"","field":"ExtAltText"},{"type":"string","optional":true,"doc":"Returns the URL mentioned in the tweet.","field":"URL"},{"type":"string","optional":true,"doc":"Returns the URL mentioned in the tweet.","field":"Text"},{"type":"string","optional":true,"doc":"Returns the expanded URL if mentioned URL is shorten.","field":"ExpandedURL"},{"type":"int32","optional":true,"doc":"Returns the index of the start character of the URL mentioned in the tweet.","field":"Start"},{"type":"int32","optional":true,"doc":"Returns the index of the end character of the URL mentioned in the tweet.","field":"End"},{"type":"string","optional":true,"doc":"Returns the display URL if mentioned URL is shorten.","field":"DisplayURL"}],"optional":false,"name":"com.github.jcustenborder.kafka.connect.twitter.MediaEntity","doc":""},"optional":true,"doc":"Returns an array of MediaEntities if medias are available in the tweet.","field":"MediaEntities"},{"type":"array","items":{"type":"struct","fields":[{"type":"int32","optional":true,"doc":"Returns the index of the start character of the symbol.","field":"Start"},{"type":"int32","optional":true,"doc":"Returns the index of the end character of the symbol.","field":"End"},{"type":"string","optional":true,"doc":"Returns the text of the entity","field":"Text"}],"optional":false,"name":"com.github.jcustenborder.kafka.connect.twitter.SymbolEntity","doc":""},"optional":true,"doc":"Returns an array of SymbolEntities if medias are available in the tweet.","field":"SymbolEntities"},{"type":"array","items":{"type":"struct","fields":[{"type":"string","optional":true,"doc":"Returns the URL mentioned in the tweet.","field":"URL"},{"type":"string","optional":true,"doc":"Returns the URL mentioned in the tweet.","field":"Text"},{"type":"string","optional":true,"doc":"Returns the expanded URL if mentioned URL is shorten.","field":"ExpandedURL"},{"type":"int32","optional":true,"doc":"Returns the index of the start character of the URL mentioned in the tweet.","field":"Start"},{"type":"int32","optional":true,"doc":"Returns the index of the end character of the URL mentioned in the tweet.","field":"End"},{"type":"string","optional":true,"doc":"Returns the display URL if mentioned URL is shorten.","field":"DisplayURL"}],"optional":false,"name":"com.github.jcustenborder.kafka.connect.twitter.URLEntity","doc":""},"optional":true,"doc":"Returns an array if URLEntity mentioned in the tweet.","field":"URLEntities"}],"optional":false,"name":"com.github.jcustenborder.kafka.connect.twitter.Status","doc":"Twitter status message."},"payload":{"CreatedAt":1621987964000,"Id":1397344916137721861,"Text":"*I like Dominic and he’s one of the smarter and more inspired people I met in the early days of ethereum","Source":"<a href=\"https://mobile.twitter.com\" rel=\"nofollow\">Twitter Web App</a>","Truncated":false,"InReplyToStatusId":1397343881801445377,"InReplyToUserId":14654085,"InReplyToScreenName":"iamtexture","GeoLocation":null,"Place":null,"Favorited":false,"Retweeted":false,"FavoriteCount":0,"User":{"Id":14654085,"Name":"Texture","ScreenName":"iamtexture","Location":"Earth","Description":"#Ethereum ⬨ - Views expressed are not my own. Temporary manifestation of the universal eternal godhead. Book character. CEO of NFTs and NFT related activities.","ContributorsEnabled":false,"ProfileImageURL":"http://pbs.twimg.com/profile_images/1359564521703145480/nvZBVGGm_normal.png","BiggerProfileImageURL":"http://pbs.twimg.com/profile_images/1359564521703145480/nvZBVGGm_bigger.png","MiniProfileImageURL":"http://pbs.twimg.com/profile_images/1359564521703145480/nvZBVGGm_mini.png","OriginalProfileImageURL":"http://pbs.twimg.com/profile_images/1359564521703145480/nvZBVGGm.png","ProfileImageURLHttps":"https://pbs.twimg.com/profile_images/1359564521703145480/nvZBVGGm_normal.png","BiggerProfileImageURLHttps":"https://pbs.twimg.com/profile_images/1359564521703145480/nvZBVGGm_bigger.png","MiniProfileImageURLHttps":"https://pbs.twimg.com/profile_images/1359564521703145480/nvZBVGGm_mini.png","OriginalProfileImageURLHttps":"https://pbs.twimg.com/profile_images/1359564521703145480/nvZBVGGm.png","DefaultProfileImage":false,"URL":null,"Protected":false,"FollowersCount":3924,"ProfileBackgroundColor":"FFFFFF","ProfileTextColor":"333333","ProfileLinkColor":"42A0E8","ProfileSidebarFillColor":"DDFFCC","ProfileSidebarBorderColor":"FFFFFF","ProfileUseBackgroundImage":true,"DefaultProfile":false,"ShowAllInlineMedia":false,"FriendsCount":1239,"CreatedAt":1209950845000,"FavouritesCount":42173,"UtcOffset":-1,"TimeZone":null,"ProfileBackgroundImageURL":"http://abs.twimg.com/images/themes/theme1/bg.png","ProfileBackgroundImageUrlHttps":"https://abs.twimg.com/images/themes/theme1/bg.png","ProfileBannerURL":"https://pbs.twimg.com/profile_banners/14654085/1612980295/web","ProfileBannerRetinaURL":"https://pbs.twimg.com/profile_banners/14654085/1612980295/web_retina","ProfileBannerIPadURL":"https://pbs.twimg.com/profile_banners/14654085/1612980295/ipad","ProfileBannerIPadRetinaURL":"https://pbs.twimg.com/profile_banners/14654085/1612980295/ipad_retina","ProfileBannerMobileURL":"https://pbs.twimg.com/profile_banners/14654085/1612980295/mobile","ProfileBannerMobileRetinaURL":"https://pbs.twimg.com/profile_banners/14654085/1612980295/mobile_retina","ProfileBackgroundTiled":false,"Lang":null,"StatusesCount":23127,"GeoEnabled":true,"Verified":false,"Translator":false,"ListedCount":167,"FollowRequestSent":false,"WithheldInCountries":[]},"Retweet":false,"Contributors":[],"RetweetCount":0,"RetweetedByMe":false,"CurrentUserRetweetId":-1,"PossiblySensitive":false,"Lang":"en","WithheldInCountries":[],"HashtagEntities":[],"UserMentionEntities":[],"MediaEntities":[],"SymbolEntities":[],"URLEntities":[]}}
```