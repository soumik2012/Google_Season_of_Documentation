# Google Season of Docs 2021 Proposal: Moja Global

## Proposal Title:

Develop a content strategy to consolidate and promote documentation for the FLINT.

## About moja global:

Help fight climate change from your keyboard! Moja global provides tools for estimating emissions and removals of greenhouse gases (GHG) from the land sector. The Full Lands INtegration Tool also known as FLINT, available under MPL-2.0 License, is a platform developed by the moja global team for data collection, analysis and continuous improvement of greenhouse gas inventories related to land use and land use change at local, national and global scales.

The FLINT is based on over 20 years of experience building and operating measurement, reporting and verification (MRV) tools in Australia and Canada. The FLINT provides the capability to quickly establish an operational MRV system that can respond to policy making and planning needs. The FLINT has been designed to achieve the required accuracy through step-by-step improvements.

New FLINT users are expected to follow the IPCC guidelines to develop increasingly accurate and robust systems for greenhouse gas accounting. Users are supported by moja global, a vibrant developer community, and philanthropic partners like the UNFCCC and IUCN. We hope to grow the number of institutional users to 14 countries by 2023 and require excellent communication, education and documentation across a wide range of topics including science, policy and software development.

Moja global has hosted mentees from Google Summer of Code, Season of Documentation, The Linux Foundation and has been accepted into Outreachy. The Mentorship Working Group can put interested applicants in contact with previous mentees and mentors as needed.

## About our project

The goal of this project is to develop a content strategy that makes our different documentation types easy to navigate, access and understand. In addition to the core FLINT library, our community has developed a small ecosystem of interrelated tools and platforms. As our community has grown, so has our documentation, some of which remains siloed or inaccessible to end-users and developers who would like to work with us.

Applications have closed. Please join our join our [Slack community](https://join.slack.com/t/mojaglobal/shared_invite/zt-o6ta1ug0-rVLjAo460~d7JbZ~HpFFtw) and the #documentation channel it you wish to participate voluntarily. We need all the help we can get!

### Problem

Right now, new users experience difficulty understanding how moja global projects can be used or are interrelated. Last year, our GSoD efforts led to the development of a website to onboard new contributors, a ReadTheDocs environment for our technical documentation, and completed documentation for the moja global Reporting Tool. We’d now like to extend these resources and by adding easy to follow examples of how the FLINT works and can be used with other tools to our documentation and contributors websites.

The primary goals of this project are to:

-   Standardize documentation across all of our repositories and improve navigation between moja global projects
-   Promote user documentation on how to conduct and configure FLINT using the JSON configuration application.
-   Migrate our control flow and architecture diagrams to our contributor’s site making it accessible to a wide audience
-   Create a list of available FLINT modules for users to customise their land use change models.
-   Publish a tutorial on how to post-process the FLINT results using the moja global Visualisation and Reporting tools.

In order to facilitate a higher volume of documentation and communications, we also propose to develop a system for publishing case studies in addition to our technical documentation. We would like to add markdown documents to our contributors website for thematic discussions that appeal to a wide range of audiences.

### Scope

We seek two technical writers to work with moja global during the Google Season of Docs period. The first writer will audit our existing user, developer and policy focused documentation and develop a content strategy, including case studies, to improve the experience of new users. The second writer will work on standardising our developer documentation, setting up a static-site generator for rendering markdown documents, and migrating our existing documentation onto our Readthedocs reference site.  Both writers will be responsible for improving the user and developer experience by adding, enhancing or migrating documentation wherever needed.

#### Technical writer #1 - Harsh Bardhan Mishra

Harsh is a Computer Science and Engineering student from Chennai, India. Harsh will work with mentor Sabita Rao (technical writer by profession and GSoD '20 participant) and the moja global Technical Steering Committee (TSC) to canvas existing FLINT users, including the Mullion Group (Australia) and NRCan (Canada). Harsh will develop excellent technical communication skills and experience obtaining expert advice to create an intuitive and approachable content. During GSoD he will work closely with the TSC to develop a content strategy for case studies and news that can be incorporated into the contributors website.

Harsh will be expected to:

-   Develop a content strategy for moja global:
	- Audit the user experience when navigating moja global projects and documentation.
	 - Develop a style guide for moja global for a consistent documentation.
	 - Design overarching content workflow that ensures regular content creation, review and promotion.

-   Prepare documentation on setting up configuration files:
	 - Audit the existing documentation and regarding FLINT configuration as this is currently fragmented and difficult to navigate.
	 - Connect with subject matter experts and developers to understand the most common FLINT analyses
	 - Provide a beginner friendly tutorial for two (2) analyses (Tier-1 and Tier-3) being actively developed by moja global teams.

-   Develop case studies for moja global:
	 - Work with the TSC to prioritise a list of case studies.
	 - Collaborate with volunteers within the Documentation Working Group to develop five (5) case studies for moja global
	- Publish two (2) case studies on the contributors website

We will have multiple milestones to ensure that the technical writer has been following up with our requirements throughout the project duration.


|Milestones                                   |Achievement|Tentative Date|
|---------------------------------------------|-----------|--------------|
|Milestone 1                                  |Audit the existing documentation and meet with the Documentation Working Group.|May 10, 2021  |
|Milestone 2                                  |Deliver content strategy to TSC.|May 31, 2021  |
|Milestone 3                                  |Prepare tutorials for end-users to get started with FLINT and contribute to it.|July 25, 2021 |
|Milestone 4                                  |Prepare moja global case studies for publication.|October 10, 2021|
|Milestone 5                                  |Host the case studies on our contributors website and drive new contributions.|November 15, 2021|

#### Technical writer #2 - Open for application.

Sarthak Kundra will work with mentors Sneha Mishra (contributors website developer, ReadTheDocs maintainer, and GSoD 2020 graduate!) and Dr. Andrew O’Reilly-Nugent (TSC Director) to organise our technical and developer documentation. This will involve collaboration with the Documentation Working Group, moja global brand management partners at Climate Advisors, and the maintainers of moja global projects. Sarthak has keen eye for detail and great instincts for designing a pain-free documentation experience. Sarthak's web development experience will enabled Sarthak to take the lead on the provisioning of a static site generator, with support from other moja global developers.

-   Update the existing contributors website:
	- Work with our UI/UX and communications experts to add content and improve navigation.
	 - Use a static site generator to render markdown content and add this to the contributors websites.
	 - Document the process for adding content to the contributors website.

-   Design a documentation architecture for improved navigation between projects and their documentation
	 - Audit moja global projects repositories and associated documentation to assess conformance with the moja global style guide.
	 - Transcribe the control flow and system architecture diagrams from LucidChart to a Web Component.
	 - Design and visualize a map of moja global repositories and services.

-   Plan a migration of existing documentation onto our ReadTheDocs platform:
	 - Update the moja global [ReadTheDocs](https://github.com/moja-global/moja_global_docs) README and provide common instructions for submitting documentation.
	 - Publish the content strategy and contribution workflows for new documentation on ReadTheDocs.
	- Coordinate the migration of existing documentation for the FLINT, Reporting Tool, Taswari, GBCM and JSON application onto ReadTheDocs.

We will have multiple milestones to ensure that the technical writer has been following up with our requirements throughout the project duration.

|Milestones                                   |Achievement|Tentative Date|
|---------------------------------------------|-----------|--------------|
|Milestone 1                                  |Audit moja global project repositories and meet with Documentation Working Group|June 7, 2021  |
|Milestone 2                                  |Add markdown content to contributors website|July 26, 2021 |
|Milestone 3                                  |Publish diagrams of moja global systems and projects|September 6, 2021|
|Milestone 4                                  |Two moja global projects migrated to ReadTheDocs|October 11, 2021|
|Milestone 5                                  |Publish and promote the latest release of the contributors website|November 15, 2021|

### Performance metrics:

We will assess our GSoD 2021 impact against the the following metrics:

-   Set up Google Analytics for the contributors website and report our first SEO benchmark upon project commencement.
-   Addition of 5 new pages to the contributors website.
-   100 new visitors attracted to the contributor website and moja global community.
-   Consistent README and contribution guidelines across all moja global repositories.
-   Doubled activity in the #documentation channel, measured in terms of posts per month with bonus points for sharing a link to [docs.moja.global](http://docs.moja.global).
-   Triple the number of pull requests to [moja_global_docs](https://github.com/moja-global/moja_global_docs) from 30 to 90 and double the number of contributors from four (4) to eight (8).

### Recommended Skills:

-   Great intuition for what makes awesome documentation.
-   Familiarity with Git/Github and Markdown-documentation.
-   Familiarity with Python programming language and Sphinx documentation tools.
-   Basic skills with Front-End development and React is preferred, but not necessary.
-   Prior experience participating in an open-source community is a plus (but we welcome new contributors too!).
-   Experience with land use change monitoring and emissions accounting or desire to learn!

### Volunteers:

-   Mohammad Warid, to help develop a user-experience (UX) plan and provide support with web development contributions.
-   Shubham Karande, to assist with systems documentation, mapping the existing moja global repositories and unearthing the technical aspects of FLINT.
-   Mohit Kumar, to work with Technical Writer #1 on developing case studies with the FLINT and providing inputs for the end user documentation.

### Project budgets:

|Budget Item                                  |Amount |Running Total|Notes/Justification                                                            |
|---------------------------------------------|-------|-------------|-------------------------------------------------------------------------------|
|Technical Writers                            |$10,000|$10,000      |2 Technical Writers x $5000 each                                               |
|Volunteers                                   |$500   |$11,500      |3 Volunteer Stipends x $500 each                                               |
|Project T-Shirts                             |$150   |$11,650      |Amount includes T-Shirt and shipping costs                                     |
|In kind contribution from moja global mentors|$0     |$11,650      |Involvement of 3 - 10 moja global representatives, developers and contributors.|
