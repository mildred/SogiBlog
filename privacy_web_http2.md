A blueprint for enchanced privacy on the Web with HTTP2
=======================================================

Privacy breach comes from HTTP round trips. Especially [fingerprinting](https://panopticlick.eff.org/). When you are just requesting a single URL, you can use CURL, wget or any HTTP library. You can customize the headers you send so you are not unique on the Web. It doesn't matter if there is Javascript running in your browser having access to personal data as long as it don't communicate it back to the server.

HTTP2 also adds HTTP Push, a new feature that could be used to increase privacy. With any request, the server can answer with multiple responses, avoiding the round trip.

In this way, there are a few way for websites (that are to be opposed to web application) to respect your privacy:

- When you request a URL, the server should send you the response with any other resources required to render the page
- The browser should refrain from requesting other resources it needs. Much like in HTTPS, the browser will refrain from loading HTTP resources.
- Javascript can run unrestricted apart from:
    - the scripts should not be allowed to change the link href or the form actions
    - the scripts should not be allowed to add new resources to be fetched to the page
    - the scripts should not be allowed to make any kind of network request
- If the script change the link or form URL, and when the user submit the form or clicks the link, he MUST be warned by a mandatory dialog like in the good old days

    > The page is requesting information to be sent to the server, do you allow?
    >
    > [Review] [Yes] [No]

- If the script attempts to make a network request, the user MUST be warned:

    > The page is requesting a communication channel. This can be used to leak personal data. Do you want to allow?
    >
    > [Yes] [No]

Basically, we must separate the web in two different things:

- Applications that uses javascript and networking to do their basic job. They should be whitelisted before the user can execute them as they might be potentially harmful. Once whitelisted, they should not be restricted in what they can do.

- Web documents: blogs, articles. These pages are just there to display content and are not there to perform computing tasks. They should be restricted in everything that can render them harmful to the user. If they request more priviledges, the user should be warned and this dialog must not be cancellable. The web developper should write the page with the assumption that the dialog will be there if they do nasty things.

If not for the web, perhaps this kind of practices could be used for websites that say they respect do not track, and by browsers like the Tor browser.