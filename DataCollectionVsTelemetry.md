# Data Collection vs Telemetry in Audacity

Audacity PR #835 is a mess on many levels. Problems that exist with this PR are
stated reasons for implementing telemetry:

- Lack of stability information.
- Unknown userbase size.
- Development and support decisions.
- Resolving a known issue with unknown impact on users.

In addition, the stated information that will be gathered via a telemetry
interface are stated as:

- Application open information (Yandex)
- Session start / end events (Google)
- Debug information / errors (Google)
- File format information (Google)
- Operating System Version (Google)
- Audacity version (Google)
- Other tools used by the user (Google)
- IP Address (Google & Yandex)

The vast majority of this information, however does not need to be gathered
using telemetry, and in fact, using telemetry would likely lead  to mis-leading
information or a lot of unusable information being gathered that would not
assist the development process.

Before going into the reasons telemetry is not appropriate for addressing the
majority of this information, consideration should be given to how telemetry is
generally used in large open source projects.

## Telemetry in Open Source

There are projects that use Telemetry.  However, in many cases the
implementation of Telemetry is well documented, and frequently goes through an
exhaustive approval process before being implemented.

The following provides an overview of telemetry handling in several open source
projects.

### Case: The Linux Foundation

**The Linux Foundation** introduced their [Telemetry data collection and usage
policy](https://www.linuxfoundation.org/telemetry-data-policy/) in October 
of 2019.  The policy makes it clear that there are many items that need to be
considered before implementations are approved.  These considerations include
(but are not limited to):

1. Individual data privacy: does the data track or identify the user? If it
   doesn't could it still include personal information that is subject to laws
   and regulations?

2. Data confidentiality: does the data result in any business-sensitive
   information being sent to the project, or a third party? If there was a
   consent dialog, what guarantees that the user was authorized to grant
   consent?

3. Awareness of collection: does the software guarantee that no data is gathered
   before consent has been given?  Is it opt-out or opt-in? Can the consent
   notice be bypassed?

4. Security of the collection mechanism: does the "phone home" mechanism itself
   open security vulnerabilities to the application? Is there still a
   vulnerability if the user does not opt-in to the data gathering?

In order to deal with all of this information, the Linux Foundation requires
member applications submit to a review by providing:

- The specific data to be collected.
- Demonstrate the data is fully anonymized, and does not contain any data about
  an end user, or information that could be sensitive.
- The manner in which users are notified of the details of the data collection,
  use and distribution. Meeting requirement of consent before information is
  gathered.
- The manner in which the information is to be stored and use by the project.
- Security mechanisms used to ensure the collection of information will no
  result in unintentional collection, or security vulnerabilities.

Following all this guidance, the information is submitted to a review panel of
The Linux Foundation's legal team for review.

### Case: KDE

The KDE project introduced telemetry into their Plasma environment approximately
a year ago.  In so doing, they developed a [Telemetry
Policy](https://community.kde.org/Policies/Telemetry_Policy) that is applied to
all applications in the KDE environment.  This is a rough summary of the policy:

- Transparency:
    - Data is shared in a way that (a) is easy to understand, (b) is precise and
      complete, (c) is locally available without network connection.
    - Any changes to the gather must be highlighted during the release
      announcement.

- Control:
    - Telemetry is **always** opt-in. It is off by default, and only enabled by
      explicit user action.
    - Telemetry settings can be changed at any time, and are prominent in the
      user interface of the application.
    - Telemetry settings are independent of any other settings, features or
      functionality.
    - Applications must honor system-wide telemetry settings.
    - Detailed documentation must be provided on the operation of the Telemetry
      settings.

- Anonymity:
    - Nothing that is considered personal information is gathered, especially
      where regulations such as the GDPR are concerned.
    - No unique devices, installation or user ID's are used.
    - Data is stripped of any unnecessary details, and downsampled appropriately
      before transmission.
    - Network operation data (such as IP Addresses) are not stored with the
      telemetry data.

- Minimalism:
    - Only gather the absolutely minimal information needed.
    - Collected data must have a clear purpose.
    - Data is downsampled to the maximum extent possible before transmission.
    - Relevant correlations between individuals bit of data should be computed
      at the source whenever possible.
    - Data collection is stopped when corresponding questions have been
      answered.

- Privacy:
    - Never transmit any user content, or hints at user content (ie, file names,
      URL's, etc.)
    - Gather only system information specific to the installation / environment
      **independent** of how the application/machine/installation is used.
    - Gather statistical usage data of an installation / application.

In order to use telemetry information in a KDE project, all of the above must be
met, and a public review is passed.  This review process is repeated for every
release if there are any changes to the information being gathered.  Received
data is regularly audited for violations of this policy. If there is a
violation, the data is deleted and collection is suspended until compliance can
be re-established.  To date, only [four KDE
applications](https://community.kde.org/Telemetry_Use) use telemetry data.

### Case: GitLab

On October 23rd of 2019 GitLab announced that they were making [changes to their
Terms of Service](https://gitlab.com/gitlab-org/gitlab/-/issues/34833) that
included the addition of JavaScript on their website that would gather
information to be used by GitLab and possibly sent to third parties.  The code
would be both open source and proprietary.

They stated that they would update their Privacy Policy to indicate what
information was being gathered, and where it was being sent.

Users of GitLab immediately took to the issue that was opened to track the
announcement and wrote a number of messages of concern about the information
that was being gathered, and the lack of control about how the information was
being used.

On October 29th, GitLab sent out a letter that explained the following:

> So, what happened? In an effort to improve our user experience, we decided to implement user behavior tracking with both first and third-party technology. Clearly, our evaluation and communication processes for rolling out a change like this were lacking and we need to improve those processes. But that’s not the main thing we did wrong.

> Our main mistake was that we did not live up to our own core value of collaboration by including our users, contributors, and customers in the strategy discussion and, for that, I am truly sorry. It shouldn’t have surprised us that you have strong feelings about opt-in/opt-out decisions, first versus third-party tracking, data protection, security, deployment flexibility and many other topics, and we should have listened first.

### Other Cases

In an earlier post I documented the policies, processes and information gathered
by Firefox, including the Glean tool the Mozilla foundation specifically created
for gathering this information.

Something that wasn't discussed as a significant point was Firefox's
transparency and user's control in terms of their telemetry gathering.  Any user
can see what information has been sent to Mozilla without having to dig through
their Telemetry Portal.  Just open "about:telemetry", and the last two weeks of
pings is available for users to see and verify that none of the information
being sent to Mozilla is sensitive.  Also users have a degree of control over
what information is being gathered through a series of check boxes available in
the Privacy & Security portion of the Preferences menu.

* * * 

Also contained in the comments on this PR is a discussion with one of the main
developers of Ardour, in which we discuss the information that they gather, and
the limited usefulness of this information, and the processes and decision
making that went into adding it to Ardour.

### Telemetry Conclusion

When implemented properly in large projects, there is far more consideration
given to the process and procedures needed to make certain that appropriate
safeguards are implemented to protect end user's privacy and anonymity.  Further,
the consideration for safeguarding against sensitive information being
communicated is also required. On top of this, legal and / or public review of
telemetry information being gathered is a routine process in these projects.

When the input of the community is not considered, problems such as that issues
that GitLab created for themselves do serious damage to their perception in the
community.

## Data Collection in Open Source

There is actually a long history of data collection in open source projects.
Where data collection differs from telemetry is often in the frequency and / or
the amount of detailed information that is gathered.

### OS Hardware Profiles

Ubuntu, several of the \*BSD distributions, and others have been
building hardware profile databases for years.  This is typically done as a
one-time, completely voluntary data collection performed during the Operating
System installation.

An example of this is the [NYC\*BUG
dmesgd](https://dmesgd.nycbug.org/index.cgi?do=index) which has allowed users to
upload copies of their dmesg logs into a database since 2004.  All of the
information contained in the database is anonymized, there is no personally
identifying information.

System76 and RedHat Enterprise Linux take a different approach.  They include
tools that are used only on the basis of support needs.  These tools gather
system logs, hardware profiles, detailed configuration information and system
coredumps into a single file.  This file is then attached to support requests
through a secured system.

Ubuntu has made one of the more controversial implementations of data collection
via it's Ubuntu Report tool.  It gathers system information and sends reports to
Ubuntu including: Ubuntu version, OEM/Manufacturer, Device model number, BIOS
info, CPU details, GPU details, Installed RAM, Partition Info, Display(s)
details, Auto-login status, Live Patching status, Desktop environment, Display
server, and Timezone.

While not containing direct personally identifying information, some of the
information could be sensitive in a number of environments.  The user is
prompted during installation to disable (opt-out) of the data collection, and is
allowed to preview the data before it is sent, while making the decision. The
data gathering system is a standalone package that can be completely removed
from the system in environments where having installed would be deemed an issue.

### Application Crash Reporting

Numerous systems, including Ubuntu, RedHat Enterprise Linux, and others have
features that can be enabled to allow semi-automated submission for crash
reports with debug information.  This allows targeted collection of information
which the user likely feels more comfortable in submitting the information.

Additionally, this can be used much in the same way as the System76 and RedHat
tools for generating logs and debug information that the user can submit as part
of a support request.

### Software Popularity

Ubuntu, Debian, Arch Linux, and other distributions contain a package which is
not installed or enabled by default that allow users to send information about
the applications they are using on their system.

The information is restricted to: package name, how recently it has been used,
and how recently it has been updated.  This approach keeps to the approach of
sending minimal information, devoid of personally identifying information.

Since this software is not activated unless specifically installed by the end
user or system administrator, there is a fairly high confidence that potentially
sensitive information is not being communicated.

### Data Collection Conclusion

There is a long standing tradition of data collection when it comes to the open
source world.  However, many Linux distributions have taken a stance of data
gathering on an "as needed" basis (with the notable exception of Ubuntu).

Systems like \*BSD's NYC\*BUG are completely voluntary, and users specifically
take the time to upload log files by their own choice.

Tools like those provided by RedHat Enterprise Linux and System76 are
specifically designed to be used in tracing problems and resolving issues.  They
are only used at  the discretion of the end user or system administrator who is
seeking support.

Popularity polling is a system that has been made available in several
distributions, however it is typically not installed by default.  Instead users
that are willing to share information about their usage habits may install and
enable this functionality.

The only notable substantial outlier (at lest that I know of) with regards to
data collection is Ubuntu, which installs a default collection system.  This
system, however, allows the user to opt-out on the first boot of their system
after installation.  It also offers the user the chance to actually see the
information that is gathered to be sent to Canonical before it is sent.

## Audacity's Telemetry Justification

This brings us back to the proclaimed need for telemtry in Audacity.  First
let's look at the reasons:

### Lack of stability information

It's difficult to see how telemetry information as proposed would be able to
affect this without gathering a lot of information about the environment in
which the user is operating their system.  The declared "OS" information would
certainly be more than just the OS version?  It would be necessary to know a
lot of information about the underlying hardware configuration.  Gathering such
detailed information (such as system board, cpu, firmware version, disk
configuration, ram, etc.) could be considered by many users to be sensitive
information.

A better indicator for stability issues would be to have a reporting system that
allowed users to submit reports when instability actually occurs.  This targets
the information to specific issues, instead of having an overly broad collection
of information that makes the location of this information like finding a needle
in a haystack.

### Unknown userbase size

This is probably one of the least worthwhile arguments put forward for the
addition of telemetry information. The problem with this justification is that
the implementation of telemetry collection is broken by design.

This is a feature that the user has the choice to disable by default.  That
alone guarantees a level of inaccuracy of the information being gathered.  To
make matters worse, there is the option to compile the application without
enabling telemetry at all.

In this situation the information gathered by telemetry is very likely to be
highly inaccurate.  It will never be clear which Linux distributions disable
telemetry during package builds.  It will never be clear what percentage of the
user base opt's out of the telemetry when they are offered the option.

There are much better ways to gather this information.  As noted in the
**Software Popularity** section, many Linux distributions already collect
anonymized information regarding package installation and usage.  This
information is often freely available from the distributions.

Outside of the Linux ecosystem many of the distribution points for packages
(including, I would believe) GitHub and Audacity's own websites keep download
statistics for the package.  Many of these sites also gather client platform
information during the download process.

Overall these sources of information would paint a more accurate picture than
relying on the automated gathering of information with unknown levels of
accuracy.

### Development and support decisions.

Development and support decisions that are based on potentially highly flawed
statistics from telemetry gathering (as outlined in **Unknown userbase size**)
will likely lead to bad decision making, and only serve to disenfranchise a
portion of the current user base.

Directly polling the user base is a far better way to gather information about
the direction to take for development.  Audacity has a support forum with over
150,000 users, and several mailing lists with a high number of users.  Certainly
users that are so invested in the software to take the time to sign up for a
forum, or add their email address to an email list are the users that should be
targeted for development decisions.

Other ways of gathering information could be open, public, anonymous polls
operated from Audacity's website.

### Resolving a known issue with unknown impact on users.

This is a situation where it is difficult to understand how this information
could be used in resolving an issue.  First, it is something that really
shouldn't be the basis for making a decision on whether to resolve an issue or
not.

The primary motivation for resolving an issue should be whether or not there is
a security assessment.  Any features that is not working as designed needs to be
assessed in the light of potential risk to the stability of a users system.  This
is first and foremost.  The higher the chance that a issue can be exploited by
any potential bad actor the higher the need to fix the issue.

Basing the decision to resolve an issue on the basis of user impact is the wrong
way to go about supporting an issue.  User impact should be considered to be
equal to a threat assessment when users report the issue, and document the
severity as having a significant impact on the user base.

## The Proposed Telemetry Implementation

As stated earlier, the proposed implementation of telemetry in Audacity is
highly problematic.  But the nature of the problems with the chosen path here
are much deeper.

### Inaccurate Information

As stated above, the information being gathered is very likely to be highly
inaccurate due to numerous factors.  The number of users that will opt-out of
telemetry will never be known.  The number of downloads that have been built
with the telemetry completely disabled will never be known.

Because of these fundamental flaws, the data should be considered GIGO (garbage
in, garbage out).

### Lack of Policy Information

No consideration, or research appears to have been done to develop a Privacy
Policy that can be applied to telemetry information.  Large open source projects
like **The Linux Foundation**, **KDE**, and **Firefox** have developed processes
for the approval of telemetry implementation that involve legal and public review
before any implementation is made.

There is no process proposed by the Audacity team to ensure that the information
being gathered conforms to any regulatory requirements.

### Choice of Third Party Service Providers

Many security and privacy experts warn of using Google and Yandex's systems.
These systems are known to be used for extensive data mining.  The implementation
of these systems gather personally identifiable information (at a minimum IP
addresses) through Audacity is not an acceptable standard.

### The lack of consideration of Open Source Alternatives

At a minimum there are three Open Source alternatives that should be considered
for the implementation of a telemetry system.  The three that are known at this
time are [Mozilla Glean](https://github.com/mozilla/glean),
[Matomo](https://matomo.org/) and
[Plausible Analytics](https://github.com/plausible/analytics) all of these options are
open source licensed tools, and have statements of GDPR compliance.  Several of
them also cover CCPA and PECR compliance as well.

## Conclusion

This document has attempted to do the kind of research that the Audacity team
should have done before beginning work on implementing telemetry in Audacity.
This research has included:

- Looking at successful telemetry implementations in large open source projects
- Looking at a case where a large open source project went wrong with changes to
  their telemetry implementation
- Looking at alternative data collection techniques that could be implemented to
  largely the same effect as the proposed telemetry implementation
- Looking at the deep flaws in the proposed telemetry implementation
- Looking at the lack of communication around the proposed telemetry
  implementation
- Looking at alternative choices for implementing a telemetry system that would
  be more respectful of user privacy
- Looking at alternatives for gathering some of the information outside of using
  telemetry

Because of of all of the concerns outlined in this document, it is proposed that
the implmentation of Telemetry in Audacity be set aside until a more thorough
investigation and discussion can take place that will address many of the issues
raised in this document.

This document was researched and written in under 12 hours.  The Audacity Team
should recognize that just the amount of issues brought up with a very short
review dictates that there are issues that need a lot more research in order to
be properly and thoroughly addressed.
