Hybrid Versioning
=================

As mentioned in https://chronver.org, SemVer is becoming less and less of a good fit in this age of continuous releases. SemVer attempted to incorporate metadata into the version numbers, where:

- A major number change indicates breaking changes
- A minor number change indicates new features
- A patch number indicates that neither breakage, nor new features have occurred.

However, not everyone adheres to these rules, it's difficult to know if someone is actually using SemVer or just marking releases using 3 numbers, and the numbers themselves are rather meaningless.

[ChronVer](https://chronver.org) attempts to address the meaningless numbers issue and introduce some temporalness to versions, but in the process it throws out the metadata and removes the ability to release software on multiple tracks.

Hybrid versioning (HyVer) takes the best of SemVer and ChronVer, and also keeps the metadata separate and explicit. This makes it easier for everyone to understand what's going on whenever a new release occurs.


Format
------

    track.year.month.day.release-metadata

Where:

- `track` is a number representing the release track (much like the major version in Python). This can be 0 for the "prerelease" track.
- `year`, `month`, `day` mark the date (month and day are always 2 digits).
- `release` is the daily release number (always starting at 1). If more than one release occurs in a day, increment this (so for example the 2nd release is 2).
- `metadata` is a series of letters (always in alphabetical order) representing metadata about this particular release. If no metadata letters are needed, this field is omitted.
  - `b`: This release contains breaking changes.
  - `d`: This release contains deprecations.
  - `n`: This release contains new features/functionality/APIs

There is also a special metadata letter `z` which is used (alone) to alert users to a bad release that you cannot retract for some reason. Of course if you can retract the bad release then by all means do so. `z` is only a hack to handle poorly designed "ratchet" release systems that don't allow retraction.


Why
---

Versioning is ultimately a U/X problem. The goal is to make it as easy as possible for the user to glean the most important information about a release without having to dig through the documentation. A secondary goal is to make it easy for machines to determine which releases are safe to upgrade.

HyVer solves the major U/X issues in the following ways:

- The version string always begins with the same number of characters, following the same format: `d.dddd.dd.dd.d` This makes it very easy to scan a list of releases (for humans and machines).
- The version strings are designed to sort properly using naive alphabetic sorting routines.
- The metadata is stored separately, at the end (`-xyz`). This makes it very easy to see which versions have introduced what kinds of changes. With SemVer, you can only glean this information by comparing two versions (hopefully). With HyVer, you don't need a version to compare against in order to know what kinds of changes were introduced.
- The numbers mean something. The numbers in version `3.14.6` have no meaning, other than the implied, hopefully correct metadata. In HyVer versions, every field has a specific meaning that conveys important information to the user:
  - Release track
  - Release date
  - Important metadata


Example
-------

On July 1st, 2019, you released two versions on track 2, and 1 version on track 3:

- The first release on track 2 introduced some new backported features.
- The second release on track 2 introduced some small fixes and documentation changes, but no breakage.
- The release on track 3 introduced new features, deprecated some APIs, and also had breaking changes.

```
    2.2019.07.01.1-n
    2.2019.07.01.2
    3.2019.07.01.1-bdn
```

Later on, you realize that `3.2019.07.01.1` exposes the user passwords, but for whatever reason you're not able to retract it from release. In such a case, you tag the same code again:

```
    3.2019.07.01.1-z
```

Now your release list looks like this in alphabetical order:

```
    2.2019.07.01.1-n
    2.2019.07.01.2
    3.2019.07.01.1-bdn
    3.2019.07.01.1-z
```

People (and machines) now know to avoid `3.2019.07.01.1`.
