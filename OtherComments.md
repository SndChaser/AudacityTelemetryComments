I have been reading through this PR, and all of the comments, and agree that the idea of adding telemetry using the Google API's is extremely problematic.  There really does need to be a lot of conversation around this PR considering all of the issues that have been brought up, including (but not limited to):

- The apparent lack of following the normal process for this PR.
- The apparent lack of consideration of how this change needs to be implemented for different localities, such as the EU's GDPR, California's CCPA regulations, and other regions regulation, etc.
- Questions surrounding Google API's being used for gathering this information.
- Serious issues with privacy questions.
- Questions about the custody of this information: ie, who is going to gather and use this information?  And to what end is this information going to be used?
- Are there other options that can be implemented that will enable the same functionality without impeding on the privacy of users?

The problem with this PR is now that it is out there is a stain of mistrust on the Audacity project... Something that I would never have thought I would be saying about one of the most revered Open Source projects of all time.  (Seriously, I can't think of many projects that have had the enduring admiration of such a large audience.)

IMO - if there is not a public statement about this PR made tomorrow, I would suggest that someone participating in these comments start a **respectful** conversation on the forum, as this PR's proposals need to be discussed with the much larger user base of Audacity.

* * *

@crsib :

> Due to the large amount of worry about this PR, (which we completely understand), we want to clarify exactly what is going on:

This is a good start, with 3000 people stating they are disapproving, confused or concerned it seems that the initial proposal was less than well considered.

>     1. Telemetry is **strictly optional and disabled by default**. No data is shared unless you choose to opt-in and enable telemetry.

Strictly optional for who?  Implementing an opt-in/opt-out dialog indicates that by default you are expecting this code to be compiled into the application

>     2. Telemetry only works in the builds made by GitHub CI from the official repo (the telemetry URLs are only defined there).
> 
>     3. If you are compiling Audacity from source, we will provide a CMake option to enable the telemetry code. This option will be turned **off** by default.

This seems rather disingenuous.  The vast majority of users do not compile this code themselves.  They get it from an upstream that compiles it for them, therefore the average user will have no way of knowing if this code is compiled into the application until they start it.

> **Why have telemetry at all?**
> 
> Essentially, it’s to help us to identify product issues early:

And this is a valid use case...  HOWEVER, having a valid use case doesn't make the jump to using API's from privacy disrespecting third parties an obvious solution.  Having identified the need and use case the obvious next step should have been to start a conversation how to best implement this feature, and make certain identify:

- Impact of the feature on developers
- Impact of the feature on users
- How to safeguard user information
- What is the chain of custody for the information being gathered
- What is the life cycle for the information
- What features will be available to users to see the information that is being gathered
- What types of transparency are the developers going to maintain around this information

> Regarding the concerns about the choice of providers:
> 
>     1. We do not incorporate cross-site tracking, limiting the ability to identify the user by both Google and Yandex.

Cross-site tracking is not necessary for Google to take the information supplied to them and use it against other sources they gather information from in order to fingerprint the user. This kind of data mining operation is precisely what they have been known to do for years now.


>     3. Google would only receive:
>        a. Session start and end events;
>        b. Errors for debugging;
>        c. File formats used for import and export;
>        d. OS and Audacity versions;
>        e. Use of effects, generators, and analysis tools to prioritize future improvements;

This is more than enough information for Google to use it in their mining operations.

>     4. We will consider replacing Google and Yandex with another service if we find one that fulfills our requirements - thanks for the suggestions and keep them coming.

Did you consider anything else?  I did a quick thought experiment about other common applications that I know that gather information.  What did I come up with?  Well, I'm using FireFox, and I know they gather information all the time.

So, I did a quick double check to see what their documentation / statement is around the information they gather.  They are very clear and transparent: [Telemetry collection and deletion](https://support.mozilla.org/en-US/kb/telemetry-clientid).  I note they have some very carefully stated guidelines surrounding the gathering and use of this information:

> **How long do you keep telemetry data?**

> We keep telemetry data for 13 months.

> **How do I opt out or delete telemetry data?**

> You can opt out of sending any Firefox telemetry information at any time. If you opt out of sending telemetry data, we will also treat this as a request to delete any data we previously collected. Data will be deleted within 30 days after you opt out. 

Your proposal is that you have an on / off switch, with no indication of:

1. Where the information is stored?
2. How long the information is stored?
3. What (if anything) will be done to anonymize the information?
4. Who will have access to the information (including any third parties)?
5. If the user turns off the option later, when will their information be deleted?

Speaking to #3 above, it's definitely necessary as you have stated:

> Both services also record the IP the request is coming from.

The simple FAQ style document from Mozilla addresses a number of these concerns, and links to more in depth information regarding the gathering of this information and all of the technical details of how it is used.  This is the kind of transparency that needs to be included when addressing this kind of situation.

One of the nicer things about Mozilla's implementation is they have a whole, publicly available portal for information surrounding the telemetry they gather: [Mozilla Telemetry Portal](https://telemetry.mozilla.org/) - nothing in your proposal suggests that you would implement anything similar.

Also, very noteworthy: Mozilla has a **whole open source [SDK called GLEAN](https://mozilla.github.io/glean/dev/index.html)** for this application.  This is the kind of implementation that should have been considered before even one line of code was written to try to implement this.,

The big takeaway here, is that as a developer you have misunderstood the practice for implementing a large new feature.  From a technology standpoint it is simple.  The hard part is the **policy, procedures and processes** that are needed to support this feature.  The code is the easy part, it's working with the whole community that is difficult, BUT IT IS ABSOLUTELY CRITICAL.
