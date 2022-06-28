Applies to version: 1.1

Records are saved to and loaded from `records.json` in the [`TECHMANIA` folder](Folders_and_zips.md). This page explains the specification of this file.

The `records.json` file contains the format version and an array of individual records. Each record corresponds to one pattern under one ruleset.

# Contents of a record

* `guid`: the GUID of the pattern.
* `fingerprint`: the fingerprint of the pattern. Explained in the next section.
* `ruleset`: 0 for standard, 1 for legacy. Refer to [Rulesets](Rulesets.md).
* `score`
* `medal`: 0 for no medal, 1 for All Combo, 2 for Perfect Play, 3 for Absolute Perfect.
* `gameVersion`: the game version that this record is set on.

# Fingerprint

To ensure that changes to a pattern invalidates all records on it, each record contains a fingerprint of the pattern it's on. When reading records, the fingerprint is calculated again from the pattern and compared with the one in the record, and the record is only considered valid if they match. The fingerprint also somewhat prevents fabrication of records, though it's not its main purpose.

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

When the player achieves a valid score on a pattern, the game will check and potentially create / update the record on it.

A score is considered invalid if any of the following conditions are met:
* A custom ruleset is in use
* Any special modifier is in use
* Stage failed

When updating a record:
* If the old record's game version is 1.0 and 1.0.1, update its fingerprint and game version regardless of whether the player set a new record (see next section).
* Score and clear medal are updated separately. This means if the player achieved a better medal than the old record but not a better score, the medal would still be updated.

# Records on 1.0 and 1.0.1

Due to a design oversight, fingerprints on 1.0 and 1.0.1 did not go through the minimizing step. This meant that any change to the track format would invalidate all records on all patterns. This issue was fixed in 1.0.2.

However, the change in fingerprinting algorithm also means all records set on 1.0 and 1.0.1 would be considered invalid on 1.0.2 and onward, due to mismatching fingerprints. To work around this issue:
* When reading records on 1.0 and 1.0.1, we simply ignore the fingerprints.
* When the player completes a pattern and there's a record on it on 1.0 or 1.0.1, the game silently updates the fingerprint and game version to 1.0.2, even if the player did not set a new record.