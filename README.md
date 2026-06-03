# postybirb-to-discord-embed

It's a working title, please be patient... Also, a hasty commit, please excuse typos...

If you don't know, I'm something of an artist, and as an artist, [PostyBirb](https://github.com/Snappsu/postybirb-to-discord-embed/blob/main/github.com/mvdicarlo/postybirb) has been a godsend for helping me share stuff online. I couldn't recommend it more.

Anyways, alongside the need to post to FurAffinity, Twitter, and Bluesky, I also need to post to Discord.

PostyBirb _can_ already do that, but I'm a fiend for using Discord embeds because they look so pretty.

Unfortunately, because of how PostyBirb works and Discord's... delicate requirements for embeds, anyone who would like to make such fancy posts will likely find themselves at an impasse.

All is not lost, though!

PostyBirb allows users to send posts to custom websites!

# What Is Needed

- **A server/website you have full control over**
- A way to receive, assemble, and send the post
  - A place to host the media (if you want to have the media **in** the embed)
- A Discord webhook for receiving posts

# What I Brought

**NOTICE: At some point in the future, I'll have code here to share as a supplement. Stay tuned!**

## A server/website you have full control over

I already use Cloudflare for a lot of stuff and am very familiar with making web services.

## A way to receive, assemble, and send the post

Here comes the complicated part.

I used a Cloudflare worker for this.

Once Postybirb has posted the media to the designated sites (sans e621) it will send a POST request to the custom server. 

You can find the examples of the data here: <https://github.com/mvdicarlo/postybirb/blob/main/docs/CUSTOM_WEBSITE.md> (thank you leaftail!)

Once the server gets that, it needs to assemble and send the data into a way that Discord won't complain. 

See: [Discord Webhook Specification](https://docs.discord.com/developers/resources/webhook#execute-webhook) and [Discord Embed Object Specification](https://docs.discord.com/developers/resources/message#embed-object)

Now, the server's I ~~post~~ forward the posts to often have different rules and stuff. For example, some servers don't allow for markdown links, and other others may not allow for links to specfic sites. As such, I make different versions of posts to comply with said demands.

## A place to host the media

Using Cloudflare Workers and their R2 object storage, I made myself a little bucket to (temporarily (in theory)) host the files that PostyBirb sends. The media in the bucket can be accessed via a URL. 

**NOTE:** Discord ~~has the reading comprehension of a toddler~~ is not quite able to read the MIME data of a file. With GIFs, for example, the URL will need the .gif at the end of the URL for Discord to recognize it properly and animate it.

## A Discord webhook for receiving posts

This is the easy part. So easy, in fact, that I don't want to write it out. 

Please refer to this: <https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks>

Personally, I have a 'staging' channel that gets all my different versions of a post, and then I forward them from there.

# Recommendations

## Secure your endpoints

To my knowledge, PostyBirb can currently only add headers to the POST to custom sites. Use this to secure your endpoints! I'm not an expert in cybersecurity, so I'll leave it as a practice to the reader to figure this out. Sorry!

Feel free to poke me with recommendations or questions to put here!
