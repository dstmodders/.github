# Contributing

First off, thanks for taking the time to contribute!

Any contribution is highly appreciated no matter if this is a new feature
addition or just a small typo fix.

The following is a set of guidelines for contributing to any project hosted in
the [Depressed DST Modders][] organization on GitHub. These are mostly
guidelines, not rules. Use your best judgment, and feel free to propose changes
to this document in a pull request.

_The keywords "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119][]._

- [Code of conduct](#code-of-conduct)
- [I don't want to read this. I just have a question!](#i-dont-want-to-read-this-i-just-have-a-question)
- [What should I know before I get started?](#what-should-i-know-before-i-get-started)

  - [Mod installation](#mod-installation)
  - [Lua + LuaRocks](#lua--luarocks)
  - [Tools](#tools)

- [Style guides](#style-guides)

  - [Git commit messages][]
  - [Lua](#lua)
  - [Markdown](#markdown)
  - [XML](#xml)

- [I want to become a member!](#i-want-to-become-a-member)

  - [Workflow](#workflow)

    - [Working on an existing mod][]
    - [Creating a new mod](#creating-a-new-mod)
    - [Moving an existing mod](#moving-an-existing-mod)

  - [Communication][]
  - [Project management](#project-management)
  - [Git][]
  - [CI/CD](#cicd)
  - [Makefile](#makefile)

## Code of conduct

Any project within our team and everyone participating in any of them is
governed by the [Depressed DST Modders Code of Conduct][]. By participating, you
are expected to uphold this code. Please report unacceptable behaviour to
abuse@dstmodders.com.

## I don't want to read this. I just have a question!

The answers to the most common questions should be already covered here. If not,
don't hesitate to ask us directly.

You can reach us using:

- Discord: https://discord.gg/aypXHQYEHC
- Email: hello@dstmodders.com
- Steam Workshop: https://steamcommunity.com/app/322330/workshop/ (from any of
  our mod)

Please don't file an issue just to ask a general question. You'll get faster
results by using the resources above. But don't hesitate to open a new issue if
it's directly related to the development.

## What should I know before I get started?

If you are planning to contribute to one of the existing mods or an SDK, then
the easiest and RECOMMENDED way to set up a development environment with most of
the tools already preinstalled is to pull the following [Docker][] image and
incorporate that into your workflow.

#### Shell/Bash (Linux)

```shell
$ git clone https://github.com/dstmodders/dst-mod-sdk
$ cd ./dst-mod-sdk/
$ export DST="${HOME}/.steam/steam/steamapps/common/Don't Starve Together"
$ docker pull dstmodders/dst-mod
$ docker run --rm -itu dst-mod -v "${DST}:/opt/dont_starve/" -v "$(pwd):/opt/$(basename $(pwd))" -w "/opt/$(basename $(pwd))" dstmodders/dst-mod /bin/bash
```

#### PowerShell (Windows)

```powershell
PS C:\> git clone https://github.com/dstmodders/dst-mod-sdk
PS C:\> cd .\dst-mod-sdk\
PS C:\> $Env:DST = "C:\Program Files (x86)\Steam\steamapps\common\Don't Starve Together"
PS C:\> docker pull dstmodders/dst-mod
PS C:\> $basename = (Get-Item "${PWD}").Basename; docker run --rm -itu dst-mod -v "$($Env:DST):/opt/dont_starve/" -v "${PWD}:/opt/${basename}" -w "/opt/${basename}" dstmodders/dst-mod /bin/bash
```

### Mod installation

You MAY install any mod using one of the following methods:

- [Steam Workshop](#steam-workshop)
- [Makefile](#makefile)
- [Manually](#manually)

#### Steam Workshop

The easiest and a RECOMMENDED way to install any production-ready mod release is
by subscribing to it using the [Steam Workshop][]. You can find a link in the
"About" section in each mod repository. This way the mod will be automatically
updated upon a new version release by using the in-game "Mods" submenu.

#### Makefile

Most mods SHOULD include the [Makefile][] rule to install them to the game mods'
directory. This is the RECOMMENDED way during the development in Unix-like
environment. However, this approach requires `DST_MODS` environment variable to
point to the game mods' directory and MAY require [rsync][] to be installed as
well.

If you are following the [Docker][] development approach and have correctly
mounted the game directory, then everything SHOULD be ready out of the box.

#### Manually

If you would like to install a mod manually, for example on devices where you
don't have access to the [Steam Workshop][], you MAY:

1. Download either the source code or the workshop version from a mod repository
   "Releases" page.
2. Unpack the archive and move it to the game mods' directory.

Keep in mind, that you SHOULD manually update the mod each time a new version
has been released.

##### Steam (Linux)

The mods' directory path on Linux installed through [Steam][]:

```txt
/home/<your username>/.steam/steam/steamapps/common/Don't Starve Together/mods/
```

##### Steam (Windows)

The mods' directory path on Windows installed through [Steam][]:

```txt
C:\Program Files (x86)\Steam\steamapps\common\Don't Starve Together\mods\
```

### Lua + LuaRocks

The game engine uses the [Lua][] interpreter v5.1, so it's RECOMMENDED to use
the same version locally as well. We, generally use v5.1.5, so if you stumble
upon some compatibility issues, consider switching to that version instead.

Also, we RECOMMEND installing the latest [LuaRocks][] to install some tools used
throughout the development as well.

### Tools

Based on the type of work you are planning to do, you MAY need the following
tools:

- [ds_mod_tools][] (or our fork [dstmodders/klei-tools][])
- [ktools][] (or our fork [dstmodders/ktools][])

Tools below are RECOMMENDED to improve overall code quality and encourage
following some of the best practices:

- [Busted][]
- [EditorConfig][]
- [GNU Make][]
- [LCOV][]
- [LDoc][]
- [Luacheck][]
- [LuaCov][]
- [Prettier][]

We RECOMMEND getting familiar with these tools and integrate them into your
workflow when developing this project. Their usage is OPTIONAL but is strongly
advisable. Consider running at least code linting and tests (if there are any)
throughout the development.

## Style guides

Keep in mind, that all the suggestions are negotiable and can be changed when
a rational reason has been found.

Most repositories include an [EditorConfig][] file that describes some basic
code style rules. You SHOULD use it as the base reference.

- [Git commit messages][]
- [Lua](#lua)
- [Markdown](#markdown)
- [XML](#xml)

### Git commit messages

You SHOULD:

1. Be short and descriptive
2. Limit the first line to 72 characters or less
3. Capitalize the subject line and each paragraph ("Add feature" not "add
   feature")
4. Describe your changes in an imperative mood as if you are giving orders to
   the codebase ("Add feature..." not "Adds feature...")
5. Use the present tense ("Add feature" not "Added feature")
6. Separate subject from body with a blank line
7. End each body paragraph with a period

You SHOULD NOT:

1. End the subject line with a period
2. End body bullet points with a period

You can always look at the recent commits as a source of reference, be
consistent and use the common sense.

You MAY use the following template:

```txt
Short (72 chars or less) summary

Issues: #<issue id>

More detailed explanatory text. Wrap it to 72 characters. The blank line
separating the summary from the body is critical (unless you omit the
body entirely).

Write your commit message in the imperative: "Fix bug" and not "Fixed
bug" or "Fixes bug". This convention matches up with commit messages
generated by commands like git merge and git revert.

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Don't end bullet point with a period
- Typically a hyphen or asterisk is used for the bullet, followed by a
  single space. Use a hanging indent
```

However, in most cases just the subject line as a commit SHOULD be enough:

```txt
Short (72 chars or less) summary
```

### GitHub issues

### Lua

Besides some stylistic errors that can be caught by [Luacheck][], use
[Lua Style Guide][] as a reference throughout the project. If mentioned style
guide suggests a different approach that wasn't covered here, use the existing
code as a guide instead.

Based on the game engine, the following suggestions SHOULD apply which differ
from the mentioned guide:

- Comments SHOULD neither have a capitalized first letter, nor a trailing dot
  (unless there are multiple sentences)
- Use 100 as the maximum line length
- Use 4 spaces for indention
- Use PascalCase for classes/modules, functions and methods
- Use snake_case for the class fields and variables

#### Documentation

[LDoc][] SHOULD be used to document functions and modules. See
[LDoc Documentation][] to learn more.

Each module SHOULD have an [LDoc][] comment in the following format:

```lua
----
-- <subject | module name>.
--
-- <body | module description>.
--
-- **Source Code:** [<repository url>](<repository url>)
--
-- @author [Depressed DST Modders](https://github.com/dstmodders)
-- @copyright <current year>
-- @license <licence abbrevation>
-- @release <version>
----
```

Each class SHOULD have an [LDoc][] comment in the following format:

```lua
----
-- <subject | class name>.
--
-- <body | class description>.
--
-- **Source Code:** [<repository url>](<repository url>)
--
-- @classmod <class name>
--
-- @author [Depressed DST Modders](https://github.com/dstmodders)
-- @copyright <current year>
-- @license <licence abbrevation>
-- @release <version>
----
```

Each exposed function SHOULD have an [LDoc][] comment in one of the following
formats:

```lua
--- <subject | short function description>.
-- @tparam <type> <name> <description>
-- @treturn <type>
```

```lua
--- <subject | short function description>.
--
-- <body | long function description>.
--
-- @tparam <type> <name> <description>
-- @treturn <type>
```

Replace `< ... >` with the appropriate values.

In the end, you SHOULD:

1. Describe your modules/classes/functions in a non-imperative mood ("Adds
   feature..." not "Add feature...")
2. Use the present tense ("Adds feature" not "Added feature")
3. Use 100 as the maximum line length (links are exception)
4. Separate subject from body with a blank line (unless there is no body)
5. End each body paragraph with a period

### Markdown

All Markdown code is linted with [Prettier][], so you SHOULD follow it as well.

In general, you SHOULD:

- Use 80 as the maximum line length

### XML

All XML code is linted with [Prettier][] using [@prettier/plugin-xml][] plugin,
so you SHOULD follow it as well.

In general, you SHOULD:

- Use 4 spaces for indention
- Use 80 as the maximum line length
- Use double quotes

## I want to become a member!

Yay! Thanks for considering joining us as another depressed member.

Our policy: every job matters and any help would be appreciated. Brainstorming,
designing, coding, testing, QA, communication with end-users, etc. Even just
some extra bit of motivation helps to deal with all this... Coding is not the
centre of everything and doesn't make other members less important.

So don't hesitate to reach us about membership if you feel like it:

- Discord: https://discord.gg/aypXHQYEHC
- Email: hello@dstmodders.com
- Steam Workshop: https://steamcommunity.com/app/322330/workshop/ (from any of
  our mod)

Below you can find everything you SHOULD know about our workflow to see if it
fits you:

- [Workflow](#workflow)

  - [Working on an existing mod][]
  - [Creating a new mod](#creating-a-new-mod)
  - [Moving an existing mod](#moving-an-existing-mod)

- [Communication][]
- [Project management](#project-management)
- [Git][]
- [CI/CD](#cicd)
- [Makefile](#makefile)

### Workflow

#### Working on an existing mod

Do you have ideas on improving an already existing mod that is part of this
organization full of depressed members? Or maybe you just want to help out by
closing an open issue that others were too lazy to solve?

OMG! Thank you! Just thinking about this makes this world a little bit better
and drives away the suicide thoughts.

Working on an existing mod mostly doesn't differ from any other Open-Source
project. This is how it SHOULD be done:

1. **I create or pick an issue** that I'm either planning to work on sooner or
   later or have just decided to help with. As an example, let's take a look at
   an issue [dst-mod-auto-join#5][].

2. **I check whether this issue has already been assigned to someone else**. If
   not, I assign myself to it so others know that I've decided to deal with it.
   If yes, then I MAY ask in chat about its progress and/or just assign myself
   if I really feel like it (it's always better to have more solutions).

3. **I create an issue-specific named branch from the latest `develop` one**. If
   it's a feature request, I use the `feature` keyword as part of the branch, if
   it's a bug report, I use the `issue` instead (to learn more, see [Git][]). If
   I'm referencing an existing issue (and you are if you are reading this), I
   MAY use the `issue` keyword in both cases. For example
   ([dst-mod-auto-join#5][]):

   ```shell
   $ git checkout develop
   $ git pull origin develop
   $ git checkout -b ViktorYu234/issue-5
   ```

4. **I try to solve an issue as best as I can, commit (see
   [Git commit messages][]) and push**. Even if the issue has been solved
   only partly and I've faced some difficulties, it's still better to push my
   work as others MAY base theirs on mine. For example:

   ```shell
   $ git add .
   $ git commit
   $ git push origin ViktorYu234/issue-5
   ```

5. **If I'm happy with the result I open a Pull Request (PR). I MUST open it
   using the `develop` branch as a base**. I don't need to assign myself if I'm
   the one who is opening it and I SHOULD just wait for others to review or, at
   very least, acknowledge the fact that they are not dead yet and SHOULD at
   least try to be aware of their surroundings.

6. **I merge it myself if no one has done it yet and I'm having direct access to
   the repository**. However, if I'm the one who is merging, **I MUST be sure
   that everything is stable and has been manually tested**. I SHOULD NOT merge
   things blindly.

7. **I repeat**, if I feel like it.

#### Creating a new mod

Have an idea for a new mod? Want to feel the power of friendship instead of
developing alone? That's awesome! Everything is better when there is someone who
can back you up.

When creating a new mod, in most cases, our workflow SHOULD be the following:

1. **We communicate (see [Communication][]) with the team** when feeling to do
   something: share an idea, design, prototype, etc. No one is obligated in
   doing stuff so no one expects anything from others. If someone becomes
   interested, he/she might join you. However, don't hesitate to ask for help or
   something.

2. **We create a design prototype first**. We generally use [Figma][] as we work on
   different operating systems. Believe it or not, extra time wasted in the
   design itself always wins the time in the long run. Moreover, it's good to
   have everyone on the same page. Don't have design experience? Don't hesitate
   and ask for help. You are a designer? Join our design team as some components
   MAY already be a part of the library to speed things up.

3. **We create a repository** as soon as everyone involved is happy enough with
   it. Other members MAY wake up and become interested in the project too.
   People usually tend to have more interest after seeing what are you planning
   to do. Everyone in the [@dstmodders/maintainers][] team has direct access.
   Others are manually added to each repository for direct access if they are
   planning to maintain it in the future.

4. **We get our hands dirty and develop**. All the work is done in the `develop`
   branch, even during the initial development (before making the first
   release). We SHOULD use [GitHub Projects][] to organize all our tasks (see
   [Project management][]) and/or just communicate (see [Communication][]).
   During the initial development, we SHOULD use simple note cards in
   [GitHub Projects][] as a way to let others know about the current progress of
   any specific feature. We use branches (see [Git][]) so we wouldn't interfere
   with others work and just merge things into the `develop` branch by the end
   of the current sprint. Basically, we SHOULD:

   1. **Split the development into smaller goals (sprints)** which will be
      represented as simple notes using [GitHub Projects][] by communicating
      with each other. If we are planning ahead, we use the "Product backlog" to
      store all our sprints.

   2. **Pick the sprint as our next development goal** and move it to the
      "Current sprint" column. For example: _"Add edit/remove buttons"_.

   3. **Split our current sprint into smaller tasks** each of which is
      represented as a simple note in the "Sprint backlog" column. For example:
      _"Add delete confirmation dialog"_.

   4. **Delegate different tasks to the team members** who want to work on this
      project. This is done by editing an existing task note and prefixing it
      with the username of that team member. Every team member decides on
      his/her own what to pick and SHOULD prefix each task himself/herself. For
      example: _"@victorpopkov Add delete confirmation dialog"_.

   5. **Create a task-specific named branch from the latest `develop` one**. In
      most cases, it should include the `feature` keyword as part of the branch
      (to learn more, see [Git][]). For example:

      ```shell
      $ git checkout develop
      $ git pull origin develop
      $ git checkout -b victorpopkov/feature/add-delete-confirmation-dialog
      ```

      Or you COULD just use a card ID in the branch name as a reference:
      `victorpopkov/feature/card-70988446`

   6. **We try to solve our tasks as best as we can, commit (see
      [Git commit messages][]) and push.** For example:

      ```shell
      $ git add .
      $ git commit
      $ git push origin victorpopkov/feature/card-70988446
      ```

   7. **Merge all completed tasks by the end of the sprint into the `develop`
      branch**.

   8. **Pick a next sprint and repeat**.

5. **We make the first release and publish it** once everyone is happy. After
   this point, we just follow the [working on an existing mod][] workflow.

#### Moving an existing mod

Have an existing mod that you would like to move into this organization? Not
afraid of changing both license and author to match the organization? That's
awesome! The hardest part over.

The whole idea of this organization was to remove a one-person dependability of
each project. You know... people can't always be there 24/7 and make new
releases. We are all depressed here. Maybe tomorrow someone decides to hang
himself somewhere in the forest. But at least, the power of friendship won't let
your project die.

When moving an existing mod, our workflow SHOULD be the following:

1. **We communicate (see [Communication][]) with the team** what mod to move.
   Maybe it's a mod that already has similar functionality with one of the
   existing ones. We could just merge them to avoid having an extra project to
   maintain. However, such a scenario would be pretty rare.

2. **We create or you just move the existing repository** to this organization
   depending on whether the repository already exists or not. Maybe it exists as
   a [Steam Workshop][] release only. If that is the case, then you MAY just
   commit and push the latest release there:

   ```shell
   $ git checkout -b 'develop'
   $ git add .
   $ git commit -m 'Add latest release'
   $ git push origin develop
   ```

   Don't worry. If you are not familiar with [Git][] there is always someone to
   help you with everything. You are already a part of the team.

   From this point, you are becoming the lead maintainer of that repository and
   every Pull Request (PR) SHOULD be merged by you. However, everyone in the
   [@dstmodders/maintainers][] team will also have direct access, but we SHOULD
   respect each other and the lead maintainer.

3. **We open issues and/or PRs to meet the organization requirements** based on
   the guidelines you are reading right now. You don't have to know everything
   and can just learn things step by step. No one expects you to prepare the
   repository to meet the guidelines on your own.

4. That's it! Now we just all continue [working on an existing mod][].

### Communication

For communication between team members, we use [Slack][].

### Project management

For project management, we use [GitHub Projects][], mostly [Agile][] software
development approach along with the [Scrum][] structure. But only in theory. We
don't have any specific guidelines.

### Git

Well, we use [Git](https://git-scm.com/). What a surprise, right?

While developing, the `main` branch MAY be used for development. However, after
the first release, all development in the public repository MUST be done in the
`develop` branch instead.

Each branch MUST start with the corresponding prefix:

- **Feature**: `feature/`
- **Hotfix**: `hotfix/`
- **Issue**: `issue/`
- **Release**: `release/`

After the prefix, a label as short as possible MUST be used (or MAY be used if
there is a corresponding OPTIONAL card/issue reference number), so we could
differentiate active branches:

- **Feature**: `feature/controller-support`
- **Hotfix**: `hotfix/movement-prediction`
- **Issue**: `issue/sendrpctoserver`
- **Release**: `release/0.1.0`

You can add an OPTIONAL number pointing to the corresponding [GitHub][] issue:
`issue-1/`). Here, the label becomes OPTIONAL:

- **Feature**: `feature-42/controller-support` or `feature-42`
- **Hotfix**: `hotfix-46/movement-prediction` or `hotfix-46`
- **Issue**: `issue-1/sendrpctoserver` or `issue-1`

A shorter version without a label is RECOMMENDED when a reference number is
available.

When starting developing a certain feature or fixing a certain issue, you MUST
work in a named branch by adding your [GitHub][] username in lowercase as a
prefix:

- **Feature**: `<your username>/feature-42/controller-support` or `<your username>/feature-42`
- **Hotfix**: `<your username>/hotfix-46/movement-prediction` or `<your username>/hotfix-46`
- **Issue**: `<your username>/issue-G2/sendrpctoserver` or `<your username>/issue-G2`

This not only allows you to work peacefully on a certain feature/issue and no
one will interfere into your work but also allows you not to bother with commits
as they will be rebased into the non-prefixed branch before merging anyway.

### CI/CD

[GitHub Actions][] are used as a [Continuous Integration][] (CI) provider for
running both code linting and tests on every commit and release. The same goes
with the [Continuous Deployment][] (CD) of the latest documentation, websites or
servers.

Make sure CI/CD doesn't report any issues and consider fixing them if it does.
However, the CI/CD reports SHOULD only be the last resort as most of the issues
SHOULD be fixed locally either before making any pull requests or pushing into
the repository.

### Makefile

Most mods incorporate a [Makefile][] where the most common tasks have been
wrapped as targets. For example:

```shell
$ make help
```

```txt
Please use 'make <target>' where '<target>' is one of:

   citest                to run Busted tests for CI
   dev                   to run reinstall + ldoc + lint + testclean + test
   gitrelease            to commit modinfo.lua and CHANGELOG.md + add a new tag
   install               to install the mod
   ldoc                  to generate an LDoc documentation
   lint                  to run code linting
   luacheckglobals       to print Luacheck globals (mutating/setting)
   luacheckreadglobals   to print Luacheck read_globals (reading)
   modicon               to pack modicon
   reinstall             to uninstall and then install the mod
   release               to update version
   test                  to run Busted tests
   testclean             to clean up after tests
   testcoverage          to print the tests coverage report
   testlist              to list all existing tests
   uninstall             to uninstall the mod
   updatesdk             to update SDK to the latest version
   workshop              to prepare the Steam Workshop directory + archive
   workshopclean         to clean up Steam Workshop directory + archive
```

[@dstmodders/maintainers]: https://github.com/orgs/dstmodders/teams/maintainers
[@prettier/plugin-xml]: https://www.npmjs.com/package/@prettier/plugin-xml
[agile]: https://en.wikipedia.org/wiki/Agile_software_development
[busted]: https://olivinelabs.com/busted/
[communication]: #communication
[continuous deployment]: https://en.wikipedia.org/wiki/Continuous_deployment
[continuous integration]: https://en.wikipedia.org/wiki/Continuous_integration
[depressed dst modders code of conduct]: https://github.com/dstmodders/.github/blob/main/CODE_OF_CONDUCT.md
[depressed dst modders]: https://github.com/dstmodders
[discord]: https://discord.gg/aypXHQYEHC
[docker]: https://www.docker.com/
[ds_mod_tools]: https://github.com/kleientertainment/ds_mod_tools
[dst-mod-auto-join#5]: https://github.com/dstmodders/dst-mod-auto-join/issues/5
[dstmodders/klei-tools]: https://github.com/dstmodders/klei-tools
[dstmodders/ktools]: https://github.com/dstmodders/ktools
[editorconfig]: https://editorconfig.org/
[figma]: https://figma.com/
[git commit messages]: #git-commit-messages
[git]: #git
[github actions]: https://github.com/features/actions
[github projects]: https://github.com/features/issues/
[github]: https://github.com/
[gnu make]: https://www.gnu.org/software/make/
[ktools]: https://github.com/nsimplex/ktools
[lcov]: http://ltp.sourceforge.net/coverage/lcov.php
[ldoc documentation]: https://stevedonovan.github.io/ldoc/manual/doc.md.html
[ldoc]: https://stevedonovan.github.io/ldoc/
[lua style guide]: https://github.com/luarocks/lua-style-guide
[lua]: https://www.lua.org/
[luacheck]: https://github.com/mpeterv/luacheck
[luacov]: https://keplerproject.github.io/luacov/
[luarocks]: https://luarocks.org/
[makefile]: https://en.wikipedia.org/wiki/Makefile
[prettier]: https://prettier.io/
[project management]: #project-management
[rfc 2119]: https://www.ietf.org/rfc/rfc2119.txt
[rsync]: https://en.wikipedia.org/wiki/Rsync
[scrum]: https://en.wikipedia.org/wiki/Scrum_(software_development)
[slack]: https://dstmodders.slack.com/
[steam workshop]: https://steamcommunity.com/app/322330/workshop/
[steam]: https://store.steampowered.com/
[working on an existing mod]: #working-on-an-existing-mod
