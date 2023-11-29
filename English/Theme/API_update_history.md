# API version 2

TECHMANIA version: 2.1

Unity version: ???

* Deprecated `Paths.EscapeBackslash`, which now does nothing. Added `Paths.ForceEscapeBackslash` in case escaping is still necessary. `EscapeBackslash` was added in an age when visual elements parsed escape sequences by default; that is no longer the case.
* Switched to FMOD as the audio backend. All usages of `UnityEngine.AudioClip` and `UnityEngine.AudioSource` are effectively replaced with `FmodSoundWrap` and `FmodChannelWrap`, the new classes being designed to imitate the Unity classes as closely as possible, so older code on API version 1 still work.
    * Some APIs that use the term "clip" and "source" are renamed to "sound" and "channel" (eg. `AudioSourceManager.IsAnySourcePlaying()` --> `AudioSourceManager.IsAnySoundPlaying()`), though the old names remain for backwards compatibility.

# API version 1

TECHMANIA version: 2.0

Unity version: 2022.3.2f1

Initial version.