# brightcove-roku-feed
Updates to the default Brightcove JSON feed for Roku

### Background ###
Brightcove is a video CDN for hosting video files.
Roku is a hardware platform for building apps for watching video and playing simple games, typically connected to a television.
Anyone can easily build a Roku channel (app) using Roku's Direct Publisher option. With Direct Publisher, you just have to supply a feed file to Roku indicating where you videos live, their titles, descriptions, etc, and Roku will handle the backend code to build the channel. This feed can be JSON or MRSS; JSON is prefered.
Brightcove offers a Social Syndication API to build this feed, which will auto-update as videos change and as new videos are added to Brightcove.
Brightcove has premade feeds for different platforms (Roku, Apple TV, Fire TV, etc). As of 8/31/2021, the Roku default feed contains a few errors.
There is an option to create a custom feed, also called a Universal Feed. To specify the format of this Universal Feed, the Liquid language is used.
This repo contains Liquid formatting code for a working Roku feed with additional suggested fields.

### References ###
Brightcove Social Syndication API https://apis.support.brightcove.com/social-syndication/getting-started/public-syndication-api-overview.html
Brightcove OAuth API https://apis.support.brightcove.com/oauth/index.html --required for using Social Syndication API
Brightcove Template Feeds https://apis.support.brightcove.com/social-syndication/getting-started/sample-templates-universal-syndication.html#roku
Liquid Documentation https://shopify.github.io/liquid/filters/escape/

### How to create a universal feed ###
POST https://social.api.brightcove.com/v1/accounts/{account_id}/mrss/syndications
-H Authorization: Bearer {bearer_token}
-H Content-Type: application/json
-body:
{
  "name":"my feed name",                                #required
  "type":"universal",                                   #required
  "title":"my channel/company name"                     #required by Roku, this value maps to a field that Roku requires in the feed file
  "content_type_header":"application/json",             #required if creating a JSON feed, otherwise the Content-Type header won't match the data and Roku will throw an error
  "include_all_content":false,                          #suggested - when starting, set this to true and ignore "include_filter", but eventually you'll want to filter your content
  "include_filter":"(+tags:roku) AND (+state:ACTIVE)"   #suggested - state:ACTIVE will ignore inactive videos. I've chosen to tag content in Brightcove w/ "roku" as well
}

### How to update a universal feed ###
PUT https://social.api.brightcove.com/v1/accounts/{account_id}/mrss/syndications/{syndication_id}/template
-H Authorization: Bearer {bearer_token}
-H Content-Type: application/text
-body put your Liquid code here
