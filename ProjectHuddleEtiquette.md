# Project Huddle Etiquette

This chapter outlines the various responsibilities of the mobile developer at weekly project huddles. These recommendations are listed below in no particular order.

### Release Schedules

Estimated timeline for the release of the next beta should be clearly communicated at every weekly huddle so that the other teams, specifically the QA team, can plan their testing schedule in advance. Ideally, there should be at least one release from each platform every week and the PRs for this release should be opened no later than Thursday morning.

### Discussion of Features

New features that are coming down the pipeline should be discussed **in detail** at weekly huddles. Mobile team is responsible for reviewing the specs **before the meeting**, and making sure they have a list of questions ready for the project owner to clarify any confusing or missing details.

Feature discussion also involves two key reviews that rely on the mobile development team:

#### API Reviews

Mobile team should thoroughly review the proposed or implemented API with the infrastucture team before the feature implementation begins. If the API is not implemented yet, they should agree on a mock JSON format that covers the endpoints and object properties.

#### Design Reviews

Mobile team should review finalized or in-progress designs before the weekly huddle and leave the relevant comments on Abstract. This review should take into account: 

* Platform-specific implementation details (i.e. It would be easier to move this view elsewhere so it's easier to implement, etc.)
* API compatibility (i.e. I need to make 3 different API calls to display this information, should we revise the API or the design)
* Style modifications (i.e. Designer just introduced three different typefaces and four new colours to the app, is this normal?)

Finally, mobile team should also provide accurate estimates on how long it will take to implement discussed features, after reviewing both API and design proposals and handshaking on necessary changes.

### Discussion of Implementation Approach

Mobile team should explain implementation details of a new feature that is either in progress or completed. This will enable other teams to double check the logical approach taken by the mobile application and confirm the validity of the approach being implemented.
