Applies to version: 2.2

TECHMANIA saves/loads many types of data to/from files on the disk. Each type of file follows a specific format, which are versioned alongside the game's version. This page documents the version history of these formats.

# Version history

|Game version|`track.tech` format|`setlist.json` format|`skin.json` format|`options.json` format|`ruleset.json` format|`records.json` format|
|--|--|--|--|--|--|--|
|0.1|"2"|N/A|N/A|"1"|N/A|N/A|
|0.2-0.3|"2"|N/A|N/A|"1"|"1"|N/A|
|0.4-0.9|"2"|N/A|"1"|"1"|"1"|N/A|
|1.0-1.1|"3"|N/A|"1"|"2"|"2"|"1"|
|2.0-2.1|"3"|N/A|"2"|"3"|"2"|"1"|
|2.2|"3"|"1"|"2"|"3"|"3"|"1"|

# More notes on format versions

- Many TECHMANIA updates introduce new fields to the above files, but adding fields is backwards compatible, so there's usually no need to increment the format version. Such changes will not be recorded in this page either.
- When loading these files, if TECHMANIA sees an outdated format version, it will convert the file in-memory to the current version before using it. This may take some time, which is why TECHMANIA 1.0 suggests the user to upgrade all tracks to version "3". When saving these files, TECHMANIA will always use the current version.

# track.tech

Format version "3":
- Volume and pan are now stored as integer percents instead of floats.
- Fixed an issue where the order of pulse and lane in hold notes are the opposite of other notes.

Format version "2":
- Initial version. ("1" was used in Alpha builds and deprecated before 0.1's release)

# setlist.tech

Format version "1":
- Initial version.

# skin.json

Format version "1":
- Initial version.

# options.json

Format version "3":
- Added theme-specific options.
- Many fields related to appearance in version "2" are moved into the default theme's options.

Format version "2":
- Volumes are now stored as integer percents instead of floats.

Format version "1":
- Initial version.

# ruleset.json

Format version "3":
- Time windows and HP deltas are now a separate class, making it possible to define a different set of time windows and HP deltas for each stage in a setlist, as well as single patterns. This class is also reused in legacy ruleset overrides.

Format version "2":
- Time windows and HP deltas are now stored in arrays (one element for each judgement), instead of one field for each judgement.
- Broke `scanMargin` into `scanMarginTopBottom` and `scanMarginMiddle`, each being an array of 3 elements, corresponding to 2-lane, 3-lane and 4-lane, respectively.

Format version "1":
- Initial version.

# records.json

Format version "1":
- Initial version.