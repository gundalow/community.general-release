# Versioning, deprecation and changelogs for community.general and community.network

This is a **release candidate** (version 3.0.0-rc1).

My idea is to have this as a pinned and locked (i.e. readonly except for repo members) issue in both repos. That issue should be used to announce release-related things, like "The next xxx release is planned for <more specific date interval>" or "We plan to do a 1.1.0 release on August 15 after ansible-base 2.10 has been released". (Locking it prevents this issue to accumulate spam. It should be announce only.) The "introduction" section below should be seen in this context :)


## Introduction

This issue describes *how* and *when* community.general is released, and to announce updates to the release/versioning schedule. The next section (Next release) is always updated to contain the next version to be released. Other changes to this first post are always announced by separate posts in this issue.

## Next release

0.2.0 (end of this week)

## Releasing schedule for major and minor versions

- around 2020-06-19: 0.2.0 (a couple of days after ansible-base 2.10 beta is out; released from `master` branch)
- last week of 2020-07: 1.0.0 **major release**

From then on, release minor versions every two months, and major versions every 6 months. The precise dates will be announced on time. Before Ansible releases we might introduce additional minor releases to fix issues.

- (maybe 2020-08-xx: 1.1.0 - around ansible-base 2.10 final release; if we release 1.1.0 then, the next minor versions will get adjusted)
- 2020-09-xx: 1.1.0
- 2020-11-xx: 1.2.0
- 2021-01-xx: 2.0.0 **major release**
- mid of 2021: 3.0.0 **major release**
- beginning of 2022: 4.0.0 **major release**
- mid of 2022: 5.0.0 **major release**
- ...

If no new commit has been merged for a minor release, it will be skipped. Major versions will not be skipped.

The schedule for minor versions might be adjusted in the future (maybe once per month, maybe something else). The release schedule for patch versions (see below) would be adjusted.

## Releasing schedule for patch versions

- Patch versions `x.y.z` until the last minor release of a major release branch will only be released when necessary. The intended frequency is *never*, they are reserved for packaging failures, and fixing major breakage / security problems.
- Once the last minor release of a major release branch (usually `x.2.0`, generally `x.Y.0`) has been released, there will be bugfix releases `x.Y.z`.
- These releases will happen every two months and when necessary.

## Versioning

- `galaxy.yml` in the `master` branch will always contain the version of the **next** major or minor release. It will be updated right after a release.
- `version_added` needs to be used for every new feature and module/plugin, and needs to coincide with the next minor/major release version. (This will eventually be enforced by CI.)

## Branching

- Releasing minor and major releases is done from `stable-x` branches.
  - Shortly (usually a few hours) before a new major release, `stable-x` is branched.
- New features can be backported to the current `stable-x` release branch after `x.0.0`, and until the last minor release for `stable-x` has been released.
  - Once a new major version is released, there will be no more minor releases for previous `stable-x` branches.
  - From then on, only bugfixes are allowed for ~6 months (backporting to the current two major releases).
  - After that, only major bugfixes security fixes are allowed for another ~12 months (backporting to the current three major releases).
  - New features **must not break backwards compatibility**!
- Backporting process:
  - Until `x.2.0`, maintainers can merge backports themselves via bot. The bot will hopefully also allow to create backports automatically (if possible without conflicts). (We hope that they know the rules: no breaking of backwards compatibility!)
  - From `x.2.0` on, only RM will merge backports.

## Deprecation

- Deprecations are done by version number (not by date).
- New deprecations can be added during every minor release, under the condition that they **do not break backwards compatibility**.
- Deprecations are expected to have a deprecation cycle of at least 2 major versions (i.e. ~1 year). Maintainers can use a longer deprecation cycle if they want to support the old code for that long.

## Changelogs

- Every change that does not only affect docs or tests must have a changelog fragment.
  - Exception: fixing/extending a feature that already has a changelog fragment and has not yet been released. Such PRs **must** always link to the original PR(s) they update.
  - Use your common sense!
  - (This might change later. The `trivial` category should then be used to document changes which are not important enough to end up in the text version of the changelog.)
  - Fragments must not be added for new module PRs and new plugin PRs. The only exception are test and filter plugins: these are not automatically documented yet.
- The `(x+1).0.0` changelog continues the `x.0.0` changelog.
  - A `x.y.0` changelog with `y > 0` is not part of a changelog of a later `X.*.*` (with `X > x`) or `x,Y,*` (with `Y > y`) release.
  - A `x.y.z` changelog with `z > 0` is not part of a changelog of a later `(x+1).*.*` or `x.Y.z` (with `Y > y`) release.
  - Since everything adding to the minor/patch changelogs are backports, the same changelog fragments of these minor/patch releases will be in the next major release's changelog. (This is the same behavior as in ansible/ansible.)
- Changelogs do not contain previous major releases, and only use the `ancestor` feature (in `changelogs/changelog.yaml`) to point to the previous major release.
- Changelog fragments are removed after a release is made.
