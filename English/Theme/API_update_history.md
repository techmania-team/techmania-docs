# API version 2

TECHMANIA version: 2.1

Unity version: 2022.3.16f1

### FMOD

Switched to FMOD as the audio backend. All usages of `UnityEngine.AudioClip` and `UnityEngine.AudioSource` are replaced with `FmodSoundWrap` and `FmodChannelWrap`, new classes that wrap around FMOD classes. The wrapper classes are designed to imitate the Unity classes as closely as possible, so older code on API version 1 still mostly work.
* The class `AudioSourceManager` is renamed to `AudioManager`.
* Some APIs that use the term "clip" and "source" are renamed to "sound" and "channel" (eg. `AudioSourceManager.IsAnySourcePlaying()` --> `AudioManager.IsAnySoundPlaying()`), though the old names remain for backwards compatibility.
* Added `Options.numAudioBuffers` and `Options.useAsio`, new audio options offered by FMOD.
* The audio buffer options can no longer be adjusted at runtime. Calling `Options.ApplyAudioBufferSize()` now has no effect other than printing a warning to Unity console.
* One key difference between `AudioSource` and `FmodChannelWrap` that we can't emulate is that, once the sound stops, `AudioSource` remains valid, but `FmodChannelWrap` becomes invalid, and all operations on it will print an `ERR_INVALID_HANDLE` warning to the console. If migrating from version 1, please watch out for these warnings and adjust your code accordingly.

### Other changes

* Deprecated `Paths.EscapeBackslash`, which now does nothing. Added `Paths.ForceEscapeBackslash` in case escaping is still necessary. `EscapeBackslash` was added in an age when visual elements parsed escape sequences by default; that is no longer the case.
* Added versions of `Status.code` and `ThemeApi.Techmania.GetPlatform()` that return an enum instead of string.
* Added methods to `ThemeApi.IO` that release assets (textures, sounds, videos) loaded from files. Themes should use these methods to control their memory footprint.
* Added `ThemeApi.Techmania.InEditor()` that returns whether TECHMANIA is running inside the Unity editor.
* Added `ThemeApi.Techmania.WrapVisualElement(VisualElement)` that creates a `VisualElementWrap` object.
* Added `ThemeApi.VideoElement.SetVideoEndCallback` that allows you to receive a callback when a video finishes playing.

# API version 1

TECHMANIA version: 2.0

Unity version: 2022.3.2f1

Initial version.