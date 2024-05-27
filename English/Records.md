Applies to version: 2.2

Records are saved to and loaded from `records.json` in the [`TECHMANIA` folder](Folders_and_zips.md). This page explains the specification of this file.

The `records.json` file contains the format version, an array of pattern records, and an array of setlist records. Each record corresponds to one pattern or setlist under one ruleset.

# Contents of a pattern record

* `guid`: the GUID of the pattern.
* `fingerprint`: the fingerprint of the pattern. Explained in a later section.
* `ruleset`: 0 for standard, 1 for legacy. Refer to [Rulesets](Rulesets.md).
* `score`: this includes Fever bonus and combo bonus, if any.
* `medal`: 0 for no medal, 1 for All Combo, 2 for Perfect Play, 3 for Absolute Perfect.
* `gameVersion`: the game version that this record is set on.

Note the lack of max combo. TECHMANIA does not count combo consistently, and it has zero effect on score, therefore we decided to not include it in records.

# Contents of a setlist record

* `setlistGuid`: the GUID of the setlist.
* `patternGuids`: the GUIDs of the 4 patterns played.
* `patternFingerprints`: the fingerprints of the 4 patterns played. Explained in a later section.
* `ruleset`, `score`, `medal`, `gameVersion`: same as pattern records.

# Pattern fingerprint

To ensure that changes to a pattern invalidates all records on it, each record contains a fingerprint of the pattern(s) it's on. When reading records, the fingerprint is calculated again from the pattern and compared with the one in the record, and the record is only considered valid if they match. The fingerprint also somewhat prevents fabrication of records, though it's not its main purpose.

The algorithm to calculate the fingerprint is as follows:

1. Minimize the pattern to only include the following fields:
  * Control scheme
  * Number of playable lanes
  * Initial BPM
  * BPS
  * BPM events
  * Time stops
  * Notes
2. Serialize the pattern as a string
3. Take an SHA256 hash of the string

# Updating records

When the player achieves a valid score on a pattern or setlist, the game will check and potentially create / update the record on it.

A score is considered invalid if any of the following conditions are met:
* A custom ruleset is in use
* Any special modifier is in use
* Stage failed

When updating a record, score and clear medal are updated separately. This means if the player achieved a better medal than the old record but not a better score, the medal would still be updated.
