# Tooling and methods

We
- use the latest versions of macOS, Xcode, and Swift;
- configure environments through Bundler using SPM and Cocoapods;
- stick to the code style and use Swiftlint;
- use various code generation tools like SwiftGen, Generamba, XcodeGen, etc.;
- favor MVP or MVVM with coordinators but don’t zero in on one specific thing;
- split large monolith apps into modules;
- configure CI/CD on each project (Fastlane + Jenkins);
- write and support tests and documentation.

That’s a brief list of what we abide by in our projects. You can read about each point in more detail below.

##  Environment

- We use [Bundler](https://bundler.io/) and write down the set of basic commands in [Makefile](https://narlei.com/development/improve-your-project-using-makefile/)
- We regularly use code generation tools:
    - [SwiftGen](https://github.com/SwiftGen/SwiftGen) to provide type-safe access to resources - assets, fonts, strings;
    - [Generamba](https://github.com/strongself/Generamba) to generate code for screen modules or distinct components. [Check out](https://github.com/surfstudio/generamba-templates) the templates we use to generate MVP modules;
    - [XcodeGen](https://github.com/yonaskolb/XcodeGen) to manage the structure of a project and make it easier to add and manage new modules;
    - [SurfGen](https://github.com/surfstudio/SurfGen) to check OpenAPI 3.x specifications for accuracy and generate a model layer and service layer based on that.
- We use CocoaPods to manage third-party dependencies on our projects, but we’re already trying out SPM instead of CocoaPods on our new projects.
- To generate new projects at initialization, we have developed an in-house set of scripts to help create a project from the ground up with all the necessary environment, generic files, project structure, and certificates/profiles already in it.

## Code style

There’s a set code style we follow at the studio; you can check it out [here](https://github.com/surfstudio/SwiftCodestyle). Compliance is checked with [Swiftlint](https://github.com/realm/SwiftLint) enabled on all projects at the studio.

Any projects that differ or deviate from that, or have anything added to them, have that written down in the project documentation.

## Architecture

- We split large monolith apps into modules:
    - new apps are initially designed in a way that would split all the code into distinct modules;
    - the projects we support are gradually being split into modules;
    - we have experience in building multi-module apps on targets, CocoaPods, and SPM;
    - this kind of project structure helps set limits of visibility more distinctly for each system component, thus enabling us to split an app into independent layers more accurately and avoid conflicts when merging in large teams.
- The architectural pattern we use for specific screens is a modified version of MVP:
	- [Surf MVP](architectures/Surf_MVP.md) is the most popular pattern among the apps we’ve made;
	- to make the navigation experience more satisfactory, we use extra entities called [coordinators](architectures/Surf_MVP_Coordinators.md);
	- however, if it’s more practical to use another pattern (MVVM, VIPER, YARCH, etc.) for a specific task or an entire project, the team may make their own collective decision to go for another method.
- We split an app into independent layers:
    - each project has a distinct and encapsulated service layer;
    - we also isolate all the reusable UI components of an app into a separate module;
    - we use protocols so as not to pass dependencies explicitly, thus cutting down on the number of dependencies between system components and structuring the way they are tested.

## GitHub

In most cases, we use GitHub as a place to store and manage repositories, both for commercial and in-house projects.

The following workflow has been established:
- with each new sprint we start a new dev/sprint-N branch;
- all tasks are carried out on separate branches;
- once finished, the separate branches are merged with the dev/sprint-N branch through a pull request, provided that they have passed the mandatory code review and were approved by the required number of team members.

## CI/CD

All commercial projects have CI/CD enabled:
- after each pull request is created or in any way edited - the app is built;
- the app is uploaded to Firebase/TestFlight every time a tag is pushed to the repository;
- apps are built and uploaded on internal nodes, at the Surf contour;
- to allocate tasks, we use Jenkins, which calls specific Makefile commands within a project;
- as for projects themselves, we use Fastlane to configure specific actions in Makefile commands (building a project, testing, uploading it to Firebase/TestFlight).
  
We don’t overcrowd open-source projects with third-party dependencies. Furthermore, we avoid the Fastlane + Jenkins combination by directly calling xcodebuild commands and similar things, as well as using GitHub actions.

## Open-Source

You can check out the list of open-source libraries that we’re currently working on, supporting, or have archived [here](open-source.md).

## Best practises and useful articles

- [Surf MVP](architectures/Surf_MVP.md)
- [Surf MVP + Coordinators](architectures/Surf_MVP_Coordinators.md)