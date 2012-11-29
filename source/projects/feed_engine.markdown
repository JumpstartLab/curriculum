---
layout: page
title: FeedEngine
---

The goal of this project is to practice consuming web service APIs as well as publishing an API of your own. You will create a Tumblr or Friendfeed-like system that lets users publish a timeline of activities that they have either created on the site or that are activity imported from another third-party site such as GitHub, Twitter, and Instagram. In addition, you will provide an API for reading and writing to a user's activity stream, and publish a gem to let other application developers consume your API.

<div class="note">
<p>This project is open source. If you notice errors, typos, or have questions/suggestions, please <a href="https://github.com/JumpstartLab/curriculum/blob/master/source/projects/feed_engine.markdown">submit them to the project on Github</a>.</p>
</div>

### Learning Goals

* Allow users to consume various third-party APIs where they hold accounts.
* Publish an API, with accompanying client gem, and dog-food your API internally.
* Coordinate with project stakeholders to produce quality code and product.
* Continue to emphasize performance, UI, and overall user experience.
* Continue using TDD to drive all layers of Rails development.

### Teams and Process

Teams consist of four developers.

#### Project Starting Point

This is a greenfield project, which means you're starting from scratch, free from the confines of legacy. Don't let it overwhelm you with its raw possibility.

Choose one team member to create a fork of [JumpstartLab's FeedEngine project](https://github.com/JumpstartLab/feed_engine), set up your teammates as collaborators, and get started.

This project requires integration with many services. Here is a list of possibly helpful gems:

* [OmniAuth](https://github.com/intridea/omniauth) plus [strategies](https://github.com/intridea/omniauth/wiki/List-of-Strategies)
* [Twitter gem](https://github.com/jnunemaker/twitter)
* [Instagram Ruby client](https://github.com/Instagram/instagram-ruby-gem) and [example app](https://github.com/deeeki/instagram-ruby-gem-sample)
* [GitHub gem](https://github.com/peter-murach/github) or [Octokit](https://github.com/pengwynn/octokit)
* One of many possible choices for interacting with HTTP/REST endpoints

These are meant as suggestions and not as an authoritative laundry list.

#### Authoritative Requirements

As in the previous project, the authoritative project requirements will be maintained in your team's specific Pivotal Tracker project. Again, this means that the requirements for your team may drift slightly from others' over the duration of the project. In this case, we will provide enough story cards in Tracker to get you through the first "iteration" of the project, and then continually work with you to provide additional story cards for the next portions of work.

The process for using Pivotal Tracker has changed a bit since the previous project, to move closer to real-world use. The changes are discussed at the end of this document.

#### Shipping Early and Rapid Feedback

Rapid and frequent feedback about the work we produce is a central tenet of agile software development and lean product delivery. A common way of getting this feedback is by holding frequent show and tell sessions with the project stakeholders.

This time around, we'll be having frequent meetings with each team to review the stories that have been delivered in Tracker. If the stories meet with product stakeholder approval, they will be accepted. If not, they will be rejected and you'll get feedback about why so that you can fix them and deliver them again.

#### Pairing and Rotation

The intention remains from previous projects that most work be done in the context of pairing. In fact, more heed should be paid to the practice because with teams of four it becomes increasingly important to coordinate work streams and changes to the code base.

Although you will be choosing your pairing and schedule for the most part, please make the effort to give everyone exposure to different roles and different parts of the codebase.

#### Communication and Standups

It's difficult to put too much importance on good communication among your team members, but it can be difficult to find the right balance. You've probably encountered some of the pain points and pitfalls of communication with your pairs on previous projects. Try to treat communication with your team members as you ideally would any close personal relationship: communicate often, be proactive, actively listen, and don't take things for granted. All problems are less costly when they're tackled sooner rather than later.

Although there's no need to ensnare yourselves in heavy process, it is generally good to have regular, lightweight check-ins. It's a common practice for agile software teams to hold a session known as a "standup meeting", or just "standup", daily. It's called a standup because you should be able to have the entire thing standing comfortably. We recommend taking 5 minutes at the beginning or ending of each day to have everyone quickly cover two things: what I'm working on, and what problems have I run into getting it done.


### Base Expectations

You're expected to complete the following functional and non-functional requirements, which describe an application and accompanying gem that function as a user activity feed, API consumer, and API producer.

You will also need to focus on an excellent user experience, and your stakeholders will emphasize that when reviewing and accepting stories during the show and tell sessions.

### Functional Requirements

The application will host multiple users and their respective activity feeds. Think Tumblr, but with service API support both as a publisher and consumer.

* Feeds show posts in descending chronological order
* Feed items should be paginated to show a reasonable number of elements per page
    * Various paging and scrolling approaches may be considered valid

#### Public Visitor

As a public visitor to FeedEngine I can:

* View any public user feed at &lt;user-feed-subdomain&gt;.feedengine.com
* Indicate your appreciation for a single feed item by clicking its "Points!" link
    * Be prompted to sign up or log in when the link is clicked
    * Each feed item shows its total Points! as part of its display
* Visit `feedengine.com/login` to log in to my already existing account
    * I should be redirected to my `feedengine.com/dashboard`
* Request a password reset and receive a login link via email
* Visit `feedengine.com/signup` to create an account and set up my activity feed
    * Provide an email address, password and password confirmation, and display name
    * Account creation always results in a welcome email being sent
    * Setting up my activity feed allows me to add account info for Twitter, GitHub, and Instagram to add activities from those sources to my feed
    * Upon completion of these steps I have set up my feed and I am viewing my dashboard

#### Authenticated Feed User

As an authenticated user I can:

* View any public user feed at &lt;user-feed-subdomain&gt;.feedengine.com
    * Refeed that feed, so you republish its subsequent posts in your feed
* View any private user feed to which I have been given access
    * Visit a private feed to which I don't have access and request access
* Indicate your appreciation for a single feed item by clicking its "Points!" link
    * Each feed item shows its total Points! as part of its display
* Refeed a single feed item so that it shows up in your feed, attributed to the original creator
* Visit `/dashboard` to manipulate my feed
    * Post a new message (up to 512 characters in length), post a link to another web page with optional comment (256 characters max), or post a photo with optional comment (256 characters max)
    * View a 'Visibility' tab to make my feed public or private
        * If my feed is private
            * I will see a list of approved viewers whose approval I may revoke
            * I will see a list of pending viewer requests whose approval I may grant
    * View a notification if I have pending viewer requests with a count
    * View a 'Linked Services' tab to manage my service subscriptions
        * Unlink or link with a Twitter account
        * Unlink or link with a GitHub account
        * Unlink or link with an Instagram account
    * View a "Refeeds" tab to manage my refeeded accounts
        * See a list of feeds I am refeeding with a link to stop refeeding them
    * View an "Account" tab where I can:
      * Change my password by providing a new password and confirmation
      * Update my email address
          * A confirmation email should be sent
      * Disable my account

#### Service Integration Specifics

Working with the Twitter, GitHub, and Instagram services will generally require authentication on behalf of your users, and should import any items created after the service has been associated with the user's feed. The basic requirements for integration with each:

* Twitter
    * Import new tweets from the user's timeline [documented here](http://rdoc.info/gems/twitter) and [here](https://dev.twitter.com/docs)
    * Optionally create a tweet linking to any new post created directly on the feed
* GitHub
    * Import new events from their authenticated timeline (specifically the `CreateEvent`, `ForkEvent`, and `PushEvent` [documented here](http://developer.github.com/v3/events/types/))
* Instagram
    * Import new images from the user's feed, [documented here](http://instagr.am/developer/endpoints/users/#get_users_feed)
* FeedEngine
    * Import direct FeedEngine items (text, links, images) from a feed, [documented here]({% page_url projects/feed_engine %}) 
    Importing the latest items should be done on a sensible interval. Once an item has been imported from a third-party service for a user, it should remain in that user's feed history so long as they have a FeedEngine account.

#### Ruby Developer Consuming a FeedEngine feed

The API should produce JSON output and expect JSON payloads for creation and updating actions.

As an authenticated API user (using an API token) I can:

* Read any publically viewable feed via GET
    * Read only feed items created directly on FeedEngine (text, link, image)
* Read any private feed to which the user has access via GET
    * Read only feed items created directly on FeedEngine (text, link, image)
* Publish a text, link, or photo feed item, given the appropriate arguments, via POST
* Refeed another user's feed item, given its id

#### Friendly validation and error messages

Any submitted forms should validate the submitted data as is appropriate and display friendly error messages when allowing the user to resubmit

### Non-Functional Requirements

The non-functional requirements are based around maintaining or improve site performance as data and users scale up. The primary metric for this is in keeping response time&mdash;the elapsed time between a browser making a web request and when a "usable" page has been loaded&mdash;below some threshold. 200ms is probably a good first-pass target for the upper end of your request times.

Implement two common methods for reducing response times: caching and background workers.

#### Caching and Data Querying

Take advantage of caching techniques and efficient data queries as used in prior projects.

* data caching
* fragment caching
* page caching
* query consolidation
* database optimizations (query count, using indicies, join)

#### Out of Process Work (Background Workers)

Use background workers as appropriate. Send all email in a background process. Querying third-party APIs and processing the resulting data is very slow, and doesn't make sense to do in-process. (In-process means during the request-response cycle.) Create background jobs that can import data from third-party APIs and save the data as part of user's activity feed.

#### Dogfood API Consumption

Dogfooding is a term used to describe using internally one's own public facing product or tool in order to validate its usability and completeness. In this case, the dogfood is your published API and accompanying gem. When a user sets up their feed to include the feeds of other FeedEngine users, the system must consume those feeds via your gem.

To enforce this, the background workers **may not** connect to your application database directly, or load the Rails environment for your app. They must go through your API gem to read from and write to users' feeds.

### Example Data

You should have the following data pre-loaded in your application:

* A users with feed for each member of your team, at least one of whom is following the others. Each user's feed should be connected to Twitter,
GitHub, Instagram, and each other's FeedEngine feeds.

These details in particular should be considered fluid until the 5/18, but this list is intended to be representative.

### Extensions

In this project you as developers are expected to take a more active role in shaping the product. You are encouraged to propose additional extensions, in the form of new features and user stories describing them, to your product manager and project manager.

If you have an idea for a killer feature for your application, pitch it to your stakeholders for refinement. If they think it's awesome, they'll work with your team to create one or more user stories in Pivotal Tracker and prioritize those stories in the context of the rest of the requirements. You may be able to convince them to prioritize your feature ahead of current base requirements if it is sufficiently compelling or necessary. The product manager and project manager will work with you to determine the point value of any extension user stories.

Here are two possible extensions:

#### API Consumption Swap

You're required to provide a web service API that exposes reading and writing the activities your users publish directly on their FeedEngine feed. Your friendly competitors are, too. Partner with another team to add your projects to each others respective list of services your users may integrate with.

#### Fully-fledged API Gem

Fulfill the following, more complete API requirements:

The API should produce JSON output and expect JSON payloads for creation and updating actions.

As an anonymous API user I can:

* Read any publically viewable feed via GET

As an authenticated API user (using an API token) I can:

* Read any publically viewable feed via GET
    * Read only feed items created directly on FeedEngine (text, link, image)
    * Read all feed items (directly created and imported)
* Read any private feed to which the user has access via GET
    * Read only feed items created directly on FeedEngine (text, link, image)
    * Read all feed items (directly created and imported)
* Publish a text, link, or photo activity item, given the appropriate arguments, via POST
* Refeed another user's feed item, given its id
* Update an existing post with new information via PUT
* Make my feed public or private via PUT
* Given a private feed
    * Get a list of approved viewers via GET
    * Remove an approved viewer via DELETE
    * Add an approved viewer via POST

### Evaluation Criteria

The evaluation of the project is broken into three areas:

1. Evaluation of the user stories for each feature of the application. (44 points possible for the basic requirements, up to 12 additional extension points available)
2. Code critique and review by instructors and engineers according to rubric. (42 points possible)
3. Non-functional requirements and metrics according to rubric. (14 points possible)

The breakdown puts a lot of emphasis on the effort put into the quality of the code for your app. But also note that it's possible to earn 12 "bonus" points by building extensions. This means that "full" credit can be earned without building any extensions and that extensions can make up for points lost elsewhere.

#### Evaluation of User Stories for Base and Extensions

Each user story for the base expectations will be worth the point value they have been assigned in Pivotal Tracker. Points for a story will be awarded if that story can be exercised without crash or error.

Extension stories will also be worth their story point value in Tracker, but no story's points will count toward the total score unless all other stories of higher priority have been delivered. This means, in effect, that you must have delivered the base expecations to receive credit for any extensions.

#### Code Critique by Instructors and Engineers

The review will be performed on the Git tag `release_v1`, which must be pushed to your project's GitHub repo by 30 minutes prior to project presentations on the due date. Use the command `git tag -a release_v1` to create it and push it to your repo (with `git push --tags`).

The high-level outline for the rubric is:

1. Good object-oriented and general application design practices, such as SOLID and DRY. (10 points)
2. Use of Ruby and Rails idioms and features. (8 points)
3. Good testing practices and coverage. (10 points)
4. Meeting non-functional requirements, such as background workers and API dog-fooding. (10 points)
5. Application correctness and robustness. (4 points)

The rubric will be applied by at least one reviewer and the mean score of their totals will be used. Please review [the full rubric]({% page_url projects/feed_engine_code_review_rubric %}) and keep it in mind as you're building your app

#### Non-Functional Metrics

Here are the criteria for the non-functional requirements. Please read this rubric carefully, as not all point values are linear.

1. Performance Under Load (0-4 points)
  * 4: Average under 120ms
  * 2: Average below 200ms
  * 1: Average below 250ms
  * 0: Average over 250ms
2. User Interface & Design (0-4 points)
  * 4: WOW! This site is beautiful, functional, and clear.
  * 2: Very good design and UI that shows work far beyond dropping in a library or Bootstrap.
  * 1: Some basic design work, but doesn't show much effort beyond "ready to go" components/frameworks/etc
  * 0: The lack of design makes the site difficult / confusing to use
3. Test Coverage (0-2 points)
  * 2: Test suite covers > 85% of application code
  * 1: Test suite covers 70-84% of application code
  * 0: Test suite covers < 70% of application code
4. Code Style (0-2 points)
  * 2: Source code generates no complaints from Cane or Reek, or only whitespace/comments warning.
  * 1: Source code generates five or fewer warnings about line-length or method statement count
  * 0: Source code generates more than five warnings about line-length or method statement count

For details on how these will be evaluated, please see the [Evaluation Protocol]({% page_url projects/feed_engine_peer_review %})

### Required Workflows In Detail

#### Git/GitHub and Branching

Coordinating multiple concurrent work streams can be tricky at best without following smart source code management practices. That said, there is a tendency in the wild to over-complicate the process of managing the branching and merging of features in a project.

We're going to follow a relatively simple workflow that gets the job done well without overcomplicating it, and has an added benefit of encouraging regular communication between coordinating parts of the team.

We start with a `master` branch, in this case, the branch we inherit from the previous project, that remains stable and deployable at all times. To add a new feature, say, adding a background job for sending email, we would create a new feature branch called `email_background_jobs` and begin doing our work there.

When we think we have completed the feature, we pull in any new updates from the stable master branch into our feature branch and then push that feature branch up to GitHub. Once there, we open a GitHub pull request to pull our new feature into `master`

At this point, the other persons on the team will look at the pull request to review the code and decide if it is ready to be merged into `master`. If so, someone designated by the team can perform the merge, updating `master`. Now everyone that has work on a new feature, not yet in master, will update their in-progress feature branch to pull in the lastest code from `master`. In this way, we maintain a stable, deployable, up-to-date branch, `master`, while making progress on new features in branches that stay reasonably up to date. This reduces merge conflicts, and that is a Good Thing&trade;.

<insert image of workflow here>

For a similar but more advanced version of this workflow, see: http://reinh.com/blog/2009/03/02/a-git-workflow-for-agile-teams.html

#### Pivotal Tracker

The order that cards appear in a Tracker project indicates their priority as determined by the product manager and/or project manager. No cards should be in progress unless all cards of higher priority are completed or also in progress. At **no time** may any member of your implementation team change the prioritization of user stories in Tracker. Only the product or project managers may do so.

Any Tracker story card being worked on should be marked as in-progress by one of the members of the pair (or the solo dev) working on it. This lets other developers know not to duplicate the work going in to that card's feature. When the feature for a card is complete, that card should be marked as finished before moving on to the next card.

Although mulitple related cards may be marked as owned by a particular developer at the same time, having more than one card in progress at the same time should not be common and should likely indicate that one of the stories has been blocked by dependence on another feature.

Story cards in Tracker go through several stages: "Not Yet Started", "Started", "Finished", "Deliverd", "Accepted", and/or "Rejected".

Here are the transitions that each story card should progress through for your project:

* "Not Yet Started" - The beginning state.
* "Started" - The state of the card once someone begins working on it. From here, there are two paths:
    * Decide not to work on the card. Put the card back into "Not Yet Started", and possibly remove your ownership.
    * Complete work on the card and mark it as "Finished"
* "Finished" - this means that the card is believed to be complete and correct, and its feature branch is ready to be deployed to staging for review. If this card is dependent on other cards, they will have to be ready for delivery as well prior to deploying to a staging environment.
* "Delivered" - Put cards into this state when it has been delivered to a staging environment for review by project stakeholders. Once it has been delivered, the review the card's feature with the stakeholders, who can make one of two choices:
    * Accept the card's work as correct.
    * Reject the card's work as insufficient or incorrect. They will include the reason in the rejection.
* "Accepted" - This means the work has been completed and  approved, and is now ready to be merged into master. Open a pull request, give the work a final review as a team, and merge it in, correcting in merge conflicts that may occur. The card should not change states again.
    * If you later realize a problem with that card's work, open a new bug card.
* "Rejected" - There was some problem with the card preventing it from being accepted. The card should be restarted, putting it back into the "Started" state. Correct the problem and procede through the stages again.

Understand that this workflow is almost certainly a bit different than what you'll encounter on engineering teams, but reflects a reasonably real-world approach.
