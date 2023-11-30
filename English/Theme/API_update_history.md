# API version 2

TECHMANIA version: 2.1

Unity version: ???

* Deprecated `Paths.EscapeBackslash`, which now does nothing. Added `Paths.ForceEscapeBackslash` in case escaping is still necessary. `EscapeBackslash` was added in an age when visual elements parsed escape sequences by default; that is no longer the case.
* Switched to FMOD as the audio backend. All usages of `UnityEngine.AudioClip` and `UnityEngine.AudioSource` are effectively replaced with `FmodSoundWrap` and `FmodChannelWrap`, the new classes being designed to imitate the Unity classes as closely as possible, so older code on API version 1 still mostly work.
    * Some APIs that use the term "clip" and "source" are renamed to "sound" and "channel" (eg. `AudioSourceManager.IsAnySourcePlaying()` --> `AudioSourceManager.IsAnySoundPlaying()`), though the old names remain for backwards compatibility.
    * All APIs related to audio buffer size are now deprecated and do nothing. Changing audio buffer size crashes Unity for some reason, and Windows players can use ASIO to decrease latency anyway.
    * One key difference between `AudioSource` and `FmodChannelWrap` that we can't emulate is that, once the sound stops, `AudioSource` remains valid, but `FmodChannelWrap` becomes invalid, and all operations on it will print an `ERR_INVALID_HANDLE` warning to the console. If migrating from version 1, please watch out for these warnings and adjust your code accordingly.
* Added `Options.useAsio`, and `Options.ApplyAsio()` to apply it.

# API version 1

TECHMANIA version: 2.0

Unity version: 2022.3.2f1

Initial version.