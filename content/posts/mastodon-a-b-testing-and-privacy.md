---  
slug: mastodon-a-b-testing-and-privacy
title: Mastodon A/B Testing and Privacy
created: 2023-02-19 17:58:01.185529408+00:00
---  
Like it or not, Mastodon is competing against commercial apps that maximize their appeal using A/B testing. This is a technique that relies on logging user behaviors in a way that would be anathema to the Mastodon community. Does this put Mastodon at a permanent disadvantage, or is there a privacy-preserving, [consentful][1] way of doing A/B testing?

First let's look at how developers of apps maximize their appeal:

One way is doing user studies, recruiting small numbers of people to observe how they use proposed changes to the apps (or early mocks of the changes), and to ask them about their experiences. This can give insights for improvements that are hard to get any other way, but a rigorous recruiting process to avoid bias in the results is expensive, probably more than a Mastodon development team could afford. Just asking friends and family to try your app is going to miss out on all those people who are not like you.

The other way is by doing live A/B experiments. This means when you have some new proposed change your app to try out you can roll it out to just a fraction of your users and compare how they use it to those without the change. For web apps in particular this is easy and common.

The terminology used in A/B experiments is the same as for medical experiments. A proposed change is called a "treatment arm", which is compared to a "control arm". An actual experiment might have multiple treatment arms.

The thing that makes A/B testing problematic for Mastodon apps is how you evaluate which arm is better. Commercial apps generally do this by logging the user behavior, such as their clicking and scrolling. They can use a variety of mechanisms including JavaScript asynchronous (XHR) requests, invisible image pixels, and ping attributes on hyperlinks. All of these send the detailed logging information to the company's server in a continuous stream, associated with an identifier that represents the user.

This obviously raises privacy concerns. A privacy-respecting company will quickly aggregate the data so that it does not remain tied to an identifiable individual, but you will have to trust they are doing that. You don't know that, say. the government would not be able to subpoena all your clicks and scrolling from the company.

So, assuming Mastodon developers are not willing to do privacy invasive logging, does that mean they are at a permanent competitive disadvantage in not being able to continuously optimize their apps?

Maybe not.

Because, actually it's not the raw logging data that is interesting, it's things like "of all the times people scrolled down a page, what fraction of them played some video", which means storing just two counts: a `ScrolledCount` denominator and a `PlayedVideoAfterScrollCount` numerator. If your A/B experiment was to change the video treatment of videos below the fold, then this `PlayedVideoAfterScrollCount / ScrolledCount` might be useful to test your experimental hypothesis.

Normally these counts would be calculated on the server using the raw data streaming over from the clients.

But there is another solution.

The counts for each user could be accumulated on the client, for example in browser local storage.

[1]: https://www.consentfultech.io/
[2]: https://elk.zone/

