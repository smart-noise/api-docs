# SMARTNOISE APIS QUICK START

## Introduction

This document is meant to provide a quick start guide to using the SMARTNOISE APIs. The APIs are designed to provide a simple interface to use SmartNoise's services.

## Disclaimer

The SmartNoise APIs are currently in closed beta.
They are subject to bugs, limitations and frequent changes.
Do not hesitate to contact us if you are in the beta program and have any questions or feedback.

## Current Limitations

- You can create Lifetime Pre-Save landing pages - other types are not supported yet.
- You can only list the data for Lifetime Pre-Save landing pages - other types are not supported yet.

## Authentication

All APIs are protected by an API key. You can obtain an API key by contacting us.
Every call must include an `Authorization` header with the value `Bearer <API_KEY>`.
Your API key is private, do not share it with anyone.

## Creating a Landing Page

The general flow to create a landing page is as follows:

1. (optional) Find an artist ID
2. (optional) Add the artist to your account
3. (optional) View the data for a landing page you created in the past
4. Create the landing page

### Find an Artist ID

You can skip this step if the artist you want the landing page for is already in your account.
To find an artist ID, you can use the `POST /smartnoise.Smartnoise/SearchArtistByName` endpoint.

Here is the example curl:

```bash
  curl --location 'https://api.smart-noise.com/smartnoise.Smartnoise/SearchArtistByName' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer API_KEY' \
    --data '{
      "userId": "YOUR_USER_ID",
      "artistName": "THE_ARTIST_NAME_YOU_ARE_LOOKING_FOR"
    }'
```

If you can't find the artist you are looking for in the result, try with a more specific name.
If you still cant't find the artist, please contact us.

### Add the Artist to Your Account

You can skip this step if the artist you want the landing page for is already in your account.
To find an artist ID, you can use the `POST /smartnoise.Smartnoise/AddArtist` endpoint.

Here is the example curl:

```bash
  curl --location 'https://api.smart-noise.com/smartnoise.Smartnoise/AddArtist' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer API_KEY' \
    --data '{
      "userId": "YOUR_USER_ID",
      "artistId": "ARTIST_ID_FROM_PREVIOUS_STEP"
    }'
```

Please not that there is a limitation on the number of artists you can create on your account.
Please contact us if you need to increase this limit.

### View the Data for a Landing Page You Created in the Past

You can view the data for a landing page you created in the past by using the `POST /smartnoise.Smartnoise/GetLandingsByUrl` endpoint.
This way you can get an idea of how the data should look like when creating a new landing page.

Here is the example curl:

```bash
  curl --location 'https://api.smart-noise.com/smartnoise.Smartnoise/GetLandingsByUrl' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer API_KEY' \
    --data '{
      "userId": "YOUR_USER_ID",
      "urls": ["THE_URL_YOU_WANT_TO_GET_THE_DATA_FOR"]
    }'
```

### Create the Landing Page

For the moment you can only create Lifetime Pre-Save landing pages.
There are a lot of fields you can set and we will be updating the documentation with a detailed explanation about all of them, but for the moment in order to create a working Lifetime Pre-Save landing page you can use this example curl:

```bash
  curl --location 'https://api.smart-noise.com/smartnoise.Smartnoise/CreateLanding' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer API_KEY' \
    --data '{
        "landing": {
            "spotifyType": "SPOTIFY_TYPE",
            "spotifyId": "SPOTIFY_ENTITY_ID",
            "title": "LANDING_TITLE",
            "description": "LANDING_DESCRIPTION",
            "image": "IMAGE_URL",
            "domain": "DOMAIN_NAME",
            "pageName": "LANDING_PAGE_NAME",
            "artistId": "SPOTIFY_ENTITY_ID",
            "services": [
                {
                    "name": "spotify",
                    "url": "https://open.spotify.com/SPOTIFY_TYPE/SPOTIFY_ENTITY_ID",
                    "button": "Pre-Save",
                    "active": true,
                    "order": 1
                }
            ],
            "customerId": "YOUR_USER_ID",
            "captureEmail": "BOOLEAN",
            "notifyFans": "BOOLEAN",
            "redirectAfter": "BOOLEAN",
            "redirectAfterUrl": "REDIRECT_URL",
            "addArtistToFollow": "BOOLEAN",
            "addToPlaylist": "BOOLEAN",
            "artistName": "ARTIST_NAME",
            "trackOrAlbumName": "TRACK_OR_ALBUM_NAME",
            "appleMusicNotifyFans": "BOOLEAN"
        },
        "userId": "YOUR_USER_ID"
    }'
```

Keep in mind the final url for your landing page will be `https://DOMAIN_NAME/PAGE_NAME`.

Here is the explanation for the fields:

- `spotifyType` is one of
  - `album`
  - `artist`
  - `playlist`
  - `track`
- `spotifyId` is the id of the spotify entity you want to create the landing page for, if it's an artist you can use the /SearchArtistByName endpoint to get it.
- If you do not know the spotifyType and spotifyId you can try using a direct Spotify url (for example <https://open.spotify.com/artist/ARTIST_ID>) that you can find by googling the artist and we will try to extract the information from it.
- `title` is the title of the landing page.
- `description` is the description of the landing page.
- `image` is the image of the landing page. Please make sure it is public.
- `domain` is the domain of the landing page. The default one is nfan.link, but you can use one of your custom SmartNoise domains if they are configured.
- `pageName` is the name of the landing page.
- `artistId` is the spotify id of the artist you want to create the landing page for. If it's an artist you can use the /SearchArtistByName endpoint to get it.
- `services` is an array of services you want to show in the landing page. For the moment we support:
  - apple_music
  - anghami
  - amazon_music
  - youtube
  - youtube_music
  - twitch
  - tidal
  - spotify
  - soundcloud
  - shows
  - official_website
  - merch_store.
  - itunes_store
  - discord
  - deezer
  - chillhop
  - beatport
  - bandcamp
  - audiomack
- `customerId` is your user id.
- `captureEmail` is a boolean to enable or disable email capture.
- `notifyFans` is a boolean to enable or disable email notifications.
- `redirectAfter` is a boolean to enable or disable redirect after submission.
- `redirectAfterUrl` is the url to redirect after submission.
- `addArtistToFollow` is a boolean to enable or disable adding the artist to follow.
- `addToPlaylist` is a boolean to enable or disable adding to playlist.
- `artistName` is the name of the artist.
- `trackOrAlbumName` is the name of the track or album.
- `appleMusicNotifyFans` is a boolean to enable or disable apple music notifications.
