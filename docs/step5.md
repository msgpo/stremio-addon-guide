---
title: 5. Providing streams
---

Your add-on now features a catalog with item details. Now is the time to supply some streams.

The streams in Stremio's add-ons are just shortcuts to the real media. They do not contain any videos - they just point Stremio to the right place where these videos can be found.

Update the manifest
---

For streams to work you just need to append `streams` to your `resources` array. That's it.

The resources section of the manifest will now look like that:

```JavaScript
"resources": [
    "catalog",
    {
        "name": "meta",
        "types": ["movie"],
        "idPrefixes": ["hiwrld_"]
    },
    "stream"
]
```

Add streams
---

The `stream` resource is reached in similar way as `meta`. There is one difference though. The `id` of the stream corresponds to the video `id` and not the meta item `id`. The reason for this is that some items like `series` and `channlel` are designed to contain more than one video. Fortunately, in simple cases like yours where there are no videos, Stremio is looking for streams that match the meta `id`.

The route that Stremio will search for streams is `/stream/<type>/<video id>.json`. Basically now you will create the `/stream/movie/` directory. You will populate it with files whose names match the ID of every video you list in your catalog.

<!--DOCUSAURUS_CODE_TABS-->
<!--bash-->
```bash
mkdir -p stream/movie
```
<!--cmd-->
```batch
mkdir stream\movie
```
<!--END_DOCUSAURUS_CODE_TABS-->

You will begin with "The Wizard of Oz". It's ID is `tt0032138`. So your stream will be in file `stream/movie/tt0032138.json`.

The stream object for this particular video features torrent info hash:

```json
{
    "streams": [
        { "title": "Torrent", "infoHash": "1588987DB4C7D98F74FB436AD8FEDE1CBE9F1F63" }
    ]
}
```

Now let's create `stream/movie/tt0017136.json` for the "Metropolis" movie:

```json
{
    "streams": [
        { "title": "YouTube", "ytId": "AvtWDIZtrAE" }
    ]
}
```

Finally, you will add several streams for the jellyfish video into `stream/movie/hiwrld_jellyfish.json`:

```json
{
    "streams": [
        { "title": "Web, 3 MBps, HD", "url": "http://jell.yfish.us/media/jellyfish-3-mbps-hd-h264.mkv" },
        { "title": "Web 15 MBps, HD", "url": "http://jell.yfish.us/media/jellyfish-15-mbps-hd-h264.mkv" },
        { "title": "Web, 120 MBps, 4K", "url": "http://jell.yfish.us/media/jellyfish-120-mbps-4k-uhd-h264.mkv" }
    ]
}
```

> When your add-on is unable to provide streams for a particular video, you have to return an empty `streams` array. Otherwise and error is shown to the user.

> As you can see, the stream object can point to a variety of streams. The complete reference is [located here](https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/responses/stream.md).

Let's watch videos
---

Now you have streams defined. You can reinstall our add-on once more. Then the streams will be displayed to the right of your movie description.

![Our streams](/stremio-addon-guide/img/streams.png)
