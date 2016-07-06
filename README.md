# url-tracking-stripper

A pretty simple, open-source Chrome Extension that will remove the tracking parameters from URLs to keep them short and cleaner for sharing, bookmarking, etc.

## Utilizes:
- Chrome Extensions Runtime onMessage (chrome.runtime.onMessage): https://developer.chrome.com/extensions/runtime#event-onMessage
- Chrome Extensions Runtime sendMessage (chrome.runtime.sendMessage): https://developer.chrome.com/extensions/runtime#method-sendMessage
- Chrome Extensions Runtime getManifest (chrome.runtime.getManifest): https://developer.chrome.com/extensions/runtime#method-getManifest
- Chrome Extensions Runtime getBackgroundPage (chrome.runtime.getBackgroundPage): https://developer.chrome.com/extensions/runtime#method-getBackgroundPage
- Chrome Extensions Storage Sync (chrome.storage.sync): https://developer.chrome.com/extensions/storage
- Chrome Extensions Tabs onUpdated (chrome.tabs.onUpdated): https://developer.chrome.com/extensions/tabs#event-onUpdated
- Chrome Extensions Tabs Update (chrome.tabs.update): https://developer.chrome.com/extensions/tabs#method-update
- Chrome Extensions Tabs executeScript (chrome.tabs.executeScript): https://developer.chrome.com/extensions/tabs#method-executeScript
- Chrome Extensions webRequest & webRequestBlocking (chrome.webRequest.onBeforeRequest): https://developer.chrome.com/extensions/webRequest

- Window History pushState(): https://developer.mozilla.org/en-US/docs/Web/API/History_API#The_pushState()_method


# Stripping Methods
This Extension is free and open source, and in no way is trying to keep tabs on you. However,
different options provide different pros and cons around things like
permissions, speed and privacy. Please read about the different "Stripping Methods" avaiable
and choose which one is right for you:

1) __History Change__: This approach will simply monitor for URLs that you visit which contain tracking parameters, and
__*after the page loads*__ it will update your browser History and remove the tracking parameters from the URL for that tab.
This is pretty slick, shouldn't slow your browsing down and requires no page re-load. However, it provides absolutely
__*no increased privacy*__ for you, as any sites have already registered and tracked your visit by the time the change is
made to the URL. You can think of it as a purely cosmetic fix.

2) __Cancel and Re-Load__: This approach will attempt to cancel any Web Requests made by tabs that are found to contain
tracking parameters, and try to instead re-load the equivalent URL without the tracking parameters. While this should not
slow your general browsing since requests begin immediately *before* they are checked for containing a tracking parameter,
this approach may slow down your experience slightly on URLs that contained tracking parameters. This is because some
amount of resources will have been loaded already by the time the overall page request is cancelled, adjusted and then
re-loaded with a clean URL. Furthermore, while this approach is likely to result in increased privacy when it is able to 
removing tracking parameters before they are able to be processed, it's possible that the time between the original request
and the cancellation was enough to be tracked, so __*increased privacy is not guaranteed*__.

3) __Block and Re-Load__: This approach will evaluate any URLs you attempt to visit __*before*__ the request is actually executed.
Any URLs that appear to contain tracking will be blocked and then the equivalent URL without the tracking parameters will
be re-loaded instead.  While every effort to make the impact of this implementation minimal/unnoticeable has been made, and
Chrome has spent a lot of time optimizing this area as well, this will still add a small amount of overhead to your
web page requests. However, of the available methods, this one adds the most privacy due to the fact that tracking paramters
are stripped from URLs before they are requested.

Additional thoughts: This extension was not intended to be a catch-all for removing tracking and increasing your privacy.
Its main purpose was simply to get a bunch of noise out of your address bar, with additional privacy (depending on your
Stripping Method) being an added benefit. This extension also __*only monitors the URLs for the "main page"*__ that you are
visiting in your tabs, and does not monitor any resources that are subesquently loaded by that page that may contain
tracking parameters. If you are really interested in privacy and actually blocking all tracking requests, I highly
recommended you take a look at something that was designed with that as its goal, such as Ghostery [https://www.ghostery.com/].
