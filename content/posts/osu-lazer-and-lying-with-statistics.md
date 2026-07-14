+++
title = "osu!lazer and lying with statistics"
date = "2026-07-14"
+++
I have been an osu! player for the past 8 years, and a data nerd for 4. At some point, with a nudge from my friends, I decided to combine both ideas into one and do some data collection for osu! related to the adoption of osu!lazer, leading to the creation of [arewelazeryet](https://arewelazeryet.chiffa.lol/).

The title of the post is related to a silly phenomenon - osu! players, especially within a certain generation of players I shall not target directly, love to downplay the adoption of osu!lazer among the playerbase, so this post is an attempt to talk about the situation with more or less objective data, while also discussing this little journey in non-technical details (as the technical parts are coming in a separate blogpost)

## General methodology or "how do you know the data is correct?"
Despite saying this is a non-technical post, I shall explain the general methodology in simpler terms so that the source of all the data is known and technically-replicable.

Let's start from the simpler part - online player counts

### Player count data
Player counts are a weird statistic - technically, there are multiple ways to gather those, yet only one of them is both "allowed" and accurate. If you've ever seen [cyperdark's stats page](https://stats.osuck.net/) or [circleclickers](https://stats.circleclickers.com/), you may have seen real-time(-ish) data displayed, that also commonly doesn't match data from arewelazeryet. This is caused by an interesting situation - technically, you can get real-time user count statistics from both stable and lazer by using osu! IRC and osu-server-spectator respectively. However, [osu! IRC is known to be "leaky"](https://stats.circleclickers.com/faq.html), while [osu-server-spectator being used in unauthorized ways *will get you banned*](https://discord.com/channels/188630481301012481/1097318920991559880/1517232048035598399). So, when considering the idea, both ways were off the table immediately. Fortunately, the project that inspired me in the first place, the now-dead [stable-death by tsunyoku](https://stable-death.tsunyoku.xyz/), was relying on a much simpler API - changelogs!

If you've ever seen the [changelog page on osu.ppy.sh](https://osu.ppy.sh/home/changelog), you can see the graphs that display the current user representation across different versions of the client. These values are public under the [`/changelog`](https://osu.ppy.sh/docs/index.html#changelog11), making tracking these values basically trivial. As I've learned later on, they are also pushed only once every five minutes (for lazer, stable is more complicated), so I knew I can avoid actively polling the endpoint for no reason.

With all this in mind, I started collecting the data, but realised that being unable to get the backlog of the charts is kind of annoying, so with the help of my friend [LeaPhant](https://osu.ppy.sh/users/2368744), we got data scraped from Internet Archive's saved changelog listings over the past 3 years. This managed to be enough to get us 99% accurate data at 30 minute intervals, which was good enough for the use case, leading to the creation of the initial main page for arewelazeryet.

### Score data
The next step was to start gathering scores for various purposes. This was done through the [amazing firehose endpoint for scores at `/scores`](https://osu.ppy.sh/docs/index.html#get-scores102), which, thanks to [Badewanne3](https://osu.ppy.sh/users/2211396)'s [scores-ws](https://github.com/MaxOhn/scores-ws), became a trivial process of fetch data -> store data -> rinse and repeat.

As a way to fill the backlog, I asked my friend [respektive](https://osu.ppy.sh/users/1023489) for help, and he did not disappoint - thanks to him, we got a dump of all the scores [osu!alternative](https://discord.gg/VZWRZZXcW4) stored ever since they moved from scraping profiles to using the firehose, giving us a backfill of all scores from roughly November 10th 2025.

Currently, some of the data aggregation is displayed on a basically-brand-new [scores page](https://arewelazeryet.chiffa.lol/scores) of the website, feel free to check it out

## lazer adoption over time
A common trend among some vocal r/osugame users is saying that lazer doesn't actually have adoption. I, as someone with actual data for this, was saying for a while that this isn't exactly correct, and that the adoption is actively growing, but the issue became slightly sillier when, due to a mistake on my behalf leading to a chain of small irresponsible decisions, someone tried to spin the data around to say that all the adoption is newbie churn - an allegation that most lazer users are new players that play for a little and presumably don't stick to the game. This allegation was accompanied with a chart made by me as a way to test the data, presented here:
{{< image src="https://lazer/file.chiffa.lol/2026-07-09_17:12:37.png" alt="Here" position="center" >}}

The issue with the tweet (intentionally not linked to not provoke harassment towards the poster) is that charts are always a bit of a lie. I will use the more recent data as an example, as it's more readily available to me and reproducing the dataset collected for the screenshot above is annoying, but the general idea stays.

## Discussing and elaborating on some common takes

> Note:
> - Date range used: 2026-06-15 to 2026-07-14, a month since I started collecting data manually and not depend on osu!alternative data dumps;
> - User ID threshold for "newgens" - 38 million (join date - after June 8th 2025, as seen through user [ulian](https://osu.ppy.sh/users/38000000)). It was chosen due to being a dumb looking spike on the chart posted without my consent. Moving the threshold to the start of the spike (34M) doesn't affect values all that much;
> - Client "preference" threshold - 70% of scores on the same client.

___
### "lazer isn't growing, it's a hoax"
I'll "debunk" this really quickly, as nobody really says it ever since the data became easy to see. Lazer has been continuously growing in user share for the past two years, moving up at a decent pace and reaching its first >50% user share peak on June 5th 2026. It also averaged at above 50% user share by 18th of June, solidifying that we are, indeed, lazer yet.

{{< image src="/img/lazer/general_growth.png" position="center" >}}

> Note: I have a small gap in backtracked data, and, given it was scraped from Internet Archive, it's a miracle the gap is only about 10 days long

___
### "Only new players use lazer"
In a way, this is true. If you look at the data for the past month, the majority of scores is submitted by newer players. 

{{< image src="/img/lazer/no_threshold_past_month.png" position="center" >}}

When viewed as-is, this chart is slightly misleading - while the spike past 38M is large, it doesn't skew the statistics all that much, and the actual split is about 55:45.
{{< image src="/img/lazer/no_threshold_past_month_pie.png" position="center" >}}

What's interesting is that, if you only include older users, you can see the following picture:
{{< image src="/img/lazer/old_account_threshold_pie.png" position="center" >}}

The "newgen spike" isn't actually all that relevant! As of now, 45%+ of players are on lazer,

If you check the adoption rates over time, you may see the following:
{{< image src="/img/lazer/user_split_change.png" position="center" >}}

The user counts shift from stable to lazer both among new *and* old users. To make it easier to see, let's look at just older players.

{{< image src="/img/lazer/old_user_split_change.png" position="center" >}}

As you can fairly clearly see, osu!lazer adoption is actively growing among players who aren't complete "newgens" and likely "stuck" with the game.
___
### "But Chiffa, stable players play more"
Some people try to say that these players don't play actively, and that the core playerbase that plays the most sticks to osu!stable. In the past, I would give this sentiment a benefit of a doubt, however the current situation is way more interesting than that.

If you look at the score count split across the user IDs, you can see an interesting dynamic:

{{< image src="/img/lazer/score_count_uid_threshold.png" position="center" >}}

As you can see by this chart, 27.4% of the scores are submitted by players who registered *after* this cutoff. Which means that, if you check for more precise data, 27% of *all scores in the past month* were submitted by **just 257397 new-ish players**. At this point, you might as well accept just how large the "new player churn" is, which is to say - newbies matter and matter a lot.

If you filter out by users who have more than 300 submitted passes within the same time frame (which is a reasonable sign of activity, even if arbitrary), you can see the following:

{{< image src="/img/lazer/threshold_past_month.png" position="center" >}}

The player split still generally follows the same curve - a bell curve with the center at 18M-20M (tail end of COVID) with a rapid spike after 34M (july 2023).

If you look for the general user split, you can see that it is still a majority of lazer players.

{{< image src="/img/lazer/threshold_past_month_pie.png" position="center" >}}

As you can see, the ratio of players *grows* with a playcount cutoff, which (hypothetically, at least) shows that lazer players are more dedicated than stable players, not less.

## Thoughts
Honestly, I'm tired of this stupid culture war. I'm tired of both sides calling the other half names, I'm tired of lazer players mocking stable players for being outdated, I'm tired of stable players calling peppy a fat fuck for not updating stable, I'm tired of everyone saying things without second thought. This is what brings the divide - not the client, not the features, but people being angry for no good reason. 

I am publishing this as a way to showcase that reality is more complicated than that, and that with real data there is no reason to lie - lazer is being adopted by the community, but not at an insane pace. The game is not dying either - we've hit a new online user count peak of the past 2+ years just a few days ago, reaching 18k concurrent players at peak and averaging around 14k. The development is going great, the game really should be thriving, but if only osu! players stopped barking at each other all the time.

Oh well, to keep it serious - stay real and don't lie with statistics!
