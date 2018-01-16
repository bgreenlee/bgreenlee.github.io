--- 
title: Going "Serverless"
date: 2018-01-15 16:13:13 -0800
layout: post
---
When my friend Nelson and I set out to build our location tracking app & accompanying website, [Wanderings](https://wanderin.gs), we started down the familiar path of choosing our stack. Flask seemed like a reasonable choice. Postgres for the database. SqlAlchemy for the ORM? Sure, I guess. We figured we’d try hosting on GCP, mostly to learn more about the platform. Do we bother with containers? Meh, leave those for the youngsters. And so on.

But the more we thought about hosting our own service, the less enthused we were. The GCP costs were piling up, and we didn’t really want to be responsible for having a copy of our users’ location data. One of the driving ideas behind Wanderings was having a location tracker where you controlled your own data. Why should you trust us with a copy of it?

I should back up and talk a little about what Wanderings is and how it works.

Back in 2011 there was a small uproar when it was revealed that Apple was apparently [tracking your every move](https://arstechnica.com/gadgets/2011/04/how-apple-tracks-your-location-without-your-consent-and-why-it-matters/) with your iPhone. Out of that grew a project, [OpenPaths](http://nytlabs.com/projects/openpaths.html), that allowed you to generate your own location data, and have full control over it. The app used very little energy, so you could just start it and forget it.

However, with the release of iOS 11, OpenPaths stopped working, and its accompanying website, [openpaths.cc](https://openpaths.cc), no longer resolved. It appeared to be no longer maintained. There were some other location tracking apps out there, including [Google Timeline](https://www.google.com/maps/timeline), [Fog of World](https://fogofworld.com/en/), and [Strut](http://strutapp.com/), but none that had the combination of strong privacy guarantees , simplicity, and low-energy usage that we were looking for. So we decided to make our own.

The [Wanderings iPhone app](https://itunes.apple.com/us/app/wanderings-travel-tracking/id1292503352?ls=1&mt=8) is pretty basic. It uses iOS' significant location change service to only update your location when you move by a significant amount. It also relies on wifi and cellular for location information rather than power-hungry GPS. It does all this in the background, so you can start it once and forget it.

New location data is uploaded to your private, secured CloudKit database on iCloud, so you are the only one with access to your data.

So back to our server conundrum.

![Wanderings Screenshot](/public/images/wanderings-screenshot.png){: .embed-left }
One of our requirements was a website that would render the full heat map of your travels, let you export that data, and eventually let you do a lot of interesting things with your data. Of course, it doesn't make sense to re-download all your location data every time you visit (especially since CloudKit only lets you pull 200 records per request) so we needed some kind of cache of your data in the app, as in a database.

But we don't really want your data. And we really don't want to deal with maintaining a server somewhere, either.

Fortunately, for a few years now browsers have supported [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API/Basic_Concepts_Behind_IndexedDB), an embedded transactional database with sizable [storage limits](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API/Browser_storage_limits_and_eviction_criteria#Storage_limits). We realized if we used this as our cache, the app could be a static site ([something Nelson had explored](http://www.somebits.com/weblog/tech/good/webapps-with-json.html) years ago), which we can host cheaply anywhere. In fact, we ended up just using [GitHub Pages](https://pages.github.com/) to host the site right out of our repository.

Using a browser-embedded database has the downside of having to re-download all your data when you go to a new browser or if you've cleared your cache, but we think that's an acceptable trade-off to have complete control of your data.

So check it out and let us know what you think. Send us your bugs/feature requests/love at <support@wanderin.gs>.

Brad & Nelson