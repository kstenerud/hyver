Hybrid Versioning
=================

As mentioned in https://chronver.org, SemVer is becoming less and less of a good fit in this age of continuous releases. However, ChronVer does not handle supporting multiple tracks of your library/application (think Python 2 and 3), and does not differentiate between breaking changes and deprecations.

Hybrid versioning (HyVer) takes the best of SemVer and ChronVer to remedy this problem, and introduces other much needed features.


Format
------

    track.year.month.day.release-metadata

Where:

- `track` is a number representing the release track (much like the major version in SemVer). This can be 0 for the "prerelease" track.
- `year`, `month`, `day` mark the date (always use leading zeroes in month and day).
- `release` is the daily release number (always starting at 1). If more than one release occurs in a day, increment this (so for example the 2nd release is 2).
- `metadata` is a series of letters (always in alphabetical order) representing metadata about this particular release:
  - `b`: This release contains breaking changes.
  - `d`: This release contains deprecations.
  - `n`: This release contains new features/functionality/APIs

There is also a special metadata letter `z` which is used (alone) to alert users to a bad release that you cannot retract for some reason. Of course if you can retract the bad release then by all means do so. `z` is only a hack to handle poorly designed "ratchet" release systems that don't allow retraction.


Example
-------

On July 1st, 2019, you released two versions on track 2, and 1 version on track 3. The first release on track 2 introduced minor (but necessary) breaking changes. The release on track 3 introduced new features, deprecated some APIs and also had some unfortunately necessary breaking changes.

    2.2019.07.01.1-b
    2.2019.07.01.2
    3.2019.07.01.1-bdn

Later on, you realize that `3.2019.07.01.1` exposes the user passwords, but for whatever reason you're not able to retract it from release. In such a case, you tag the same code again:

    3.2019.07.01.1-z

Now your release list looks like this in alphabetical order:

    2.2019.07.01.1-b
    2.2019.07.01.2
    3.2019.07.01.1-bdn
    3.2019.07.01.1-z

People (and machines) now know to avoid `3.2019.07.01.1`.
