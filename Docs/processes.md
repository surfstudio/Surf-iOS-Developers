# Procedures

## The structure of Surf

Currently, Surf consists of several distinct units:
- iOS
- Android
- Flutter
- Backend and Frontend
- QA
- Design
- Analytics
- Project management
- Marketing
- DevRel
- Sales
- DevOps
- Automation and analytics
- HR and recruitment
- Administrative Office

Each unit has its own head, and some units have designated team leaders. They could be responsible for development in general or supervise groups of subordinate personnel.

Growth and development are hard to achieve unless you have a structure like this where everyone does what they are best at and an executive chain of command helps you manage the workflow more effectively. Even though we’re growing and expanding, we strive to keep the environment friendly and trust-based. No matter who comes up with it, a note-worthy initiative to adjust our workflow may be put into action. It could happen after the person has a one-on-one meeting with their superior or after the idea is approved by the committee that undertakes global initiatives in the studio.

## Teams

A classic team assigned to our average project would have the following members:
- platform developers (either 2-4 iOS and Android developers each or 3-5 Flutter developers);
    - there could be more or fewer of them depending on the scale of a project;
- analyst (BA);
- tester (QA);
- project manager (PM);
- designer.

Depending on a project, some roles might be omitted, e.g., we might have a designer or analyst on the client’s side. By contrast, new roles could be added, e.g., a back-end developer from Surf. However, in general, we strive to build a team that will assist us in running a project effectively, taking into account all of its aspects and providing the most effective solutions in terms of both technical implementation and UX.

More often than not, the back-end team is on our client’s side or provided by yet another contractor. Whatever the case, we establish communication between the front-end and back-end teams so that we can deal with everyday matters together.

Because we outsource development rather than stuff, even the smallest teams always have a project manager to deal with issues in collaboration with the client.

## Development

Our workflow may vary depending on the project. Below, you can see the traditional way of implementing an MVP app, which we follow on most of our projects:
- it all starts with a “zero sprint” where a team of analysts, designers, development team leaders and a decision maker on client’s side agree on basic requirements set for the project, create and approve the core design and technical specifications for it;
- the scope is then estimated by the team leaders of each platform, the teams are built, and budgets and deadlines are approved;
- the tasks within the entire scope are grouped into sprints, each containing a complete feature and lasting for approximately 2-3 weeks;
- that is followed by iterative work within each sprint where:
    - development team and QA accept requirements in the form of technical specs and design layouts;
    - the requirements are clarified, with all inconsistencies between the specs, layouts, and client’s expectations eliminated;
    - the team then decomposes the tasks (the top-level decomposition is performed by the team leader, and the detailed decomposition is performed by either the leader or developers, depending on the project).
    - the scope is then evaluated (depending on the project, we may use either simple hourly estimates or story points, or even poker planning);
    - that is followed by development;
    - debugging;
    - repeat all over again.

Here’s how we plan and implement tasks:
- during each sprint, the team leaders plan and clarify all the requirements in the technical specs set for the following sprint, which is followed by top-level decomposition
- once sprint tasks are done and submitted for testing - the team can already proceed to make detailed design and estimates for the tasks in the next sprint while they are debugging the code according to the feedback they receive from QA…
- …to then enter the next sprint right away

Thanks to this slight overlap between stages and the fact that the team leader is one step ahead of their team, we achieve continuous operation and avoid holdups between iterations.

After an MVP version is finished, the workflow for sprints stays the same. Only the tasks are now based not on the scope pre-defined at the start but on the feedback we receive from the client, users, business needs, and other stakeholders. The process goes through all the same stages: from analysis to development and testing.

## Quality assurance

To assure that the quality of our projects stays at the needed level, we do the following:
- code-review before any changes are merged is integral to the way all of our teams work
- technical debt is accounted for and dealt with: any potential worm corners are registered, we evaluate the time it takes to fix them, the risk of them affecting the product, and find time to eliminate the main threats to the project
- design-review helps us make sure that all layouts are correct and the UX that we have designed works properly
- routine technical retro meetings and team retro meetings give us a chance to discuss the most pressing issues, analyze the previous stages, pinpoint mistakes, and take preventive actions