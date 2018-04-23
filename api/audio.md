# Class: AudioManager

A class for creating, managing, and playing sounds. For
details about best practices when creating sound files, see
the [Creating Audio Assets Guide](../guide/audio-assets.html).

## Examples

* [Background Music](../example/sound-background/)
* [Effects](../example/sound-effect/)
* [Mute Sound](../example/sound-mute/)
* [Adjust Volume](../example/sound-volume/)
* [Adjust Background](../example/sound-background/)
* [Get and Set Track Position](../example/sound-time/)

## Methods

### new AudioManager ([options])

Parameters
:    1. `options {object}`
	     * `path {string}` ---Directory path containing audio files.
		 * `files {object}` ---Collection of audio files mapped to their options.
		 * `persist {}`

`path` is the directory containing a group of audio files,
relative to the top-level of a project. `files` is an object
mapping audio files to their options.

The `files` options object has the following properties:

 * `sources {}`
 * `background {boolean}` ---Background music
 * `loop {boolean}`
 * `volume {number}`
 * `path {string}` ---Add additional path information to the options path.

Assets flagged as `background` loop automatically. Also,
only one `background` sound can play at any given time
(playing one `background` sound stops any
currently-playing `background` sound).

For the examples on this page, we'll assume the following
directory structure of a project:

~~~
project
.
├── manifest.json
├── sdk/ -> /path/to/devkit/sdk
├── build/
├── resources/
│   └── sounds/
│       ├── music/
│       │   └── levelmusic.mp3
│       └── effect/
│           ├── boink.mp3
│           └── warble.mp3
└── src/
    └── Application.js
~~~

And to use these files with your game:

~~~
import AudioManager;

var audiomanager = new AudioManager({
  path: 'resources/sounds',
  files: {
    levelmusic: {
      path: 'music',
      volume: 0.5,
      background: true
    },
    boink: {
      path: 'effect'
    },
    warble: {
      path: 'effect',
      loop: true
    }
  }
});
~~~

### addSound (name [, options])

Parameters
:    1. `name {string}`
     2. `options {object}` ---Audio file options described above.

Add a sound to the sound group.

~~~
audiomanager.addSound('pop', {
  path: 'effect',
  background: false
});
~~~

### play (name [, options])

Parameters
:    1. `name {string}`
	 2. `options {object}`
	     * `loop {boolean} = false`
       * `time {number}`
       * `duration {number}`

Returns
:    1. `{boolean}` ---Returns `true` on success, `false` if not.

Play a sound. If it has already been preloaded, or is flagged
as a `background` sound, it will play immediately. Otherwise,
the AudioManager will load it into memory before playing.

Use the `time` and `duration` options (in seconds) to jump to any point in the track.
This is particularly useful when you're using a "soundsheet", a single sound file
containing multiple sound effects.

~~~
audiomanager.play('boink', {loop: true});
~~~

Note that the `time` option is only available for `background` sounds.

### pause (name)

Parameters
:    1. `name {string}`

Returns
:    1. `{boolean}` ---Returns `true` on success, `false` if not.

Pause a sound. The audio file is stopped at a certain point,
and restarted from that point when played again.

~~~
audiomanager.play('levelmusic');
~~~

### stop (name)

Parameters
:     1. `name {string}`

Returns
:     1. `{boolean}` ---Returns `true` on success, `false` if not.

Stop a sound. If a sound is played again, it will restart
from the beginning.

~~~
audiomanager.stop('levelmusic');
~~~

### isPaused (name)

Parameters
:     1. `name {string}`

Returns
:     1. `{boolean}` ---Returns `true` or `false`.

Returns whether or not a sound is paused.

~~~
audiomanager.isPaused('levelmusic');
~~~

### isPlaying (name)

Parameters
:     1. `name {string}`

Returns
:     1. `{boolean}` ---Returns `true` or `false`.

Returns whether or not a sound is playing. This is not the opposite of `isPaused`, because a sound that already played or hasn't started yet is neither paused nor playing.

~~~
audiomanager.isPlaying('levelmusic');
~~~

Note that this function is only available for `background` sounds.

### setVolume (name, volume)

Parameters
:    1. `name {string}`
	 2. `volume {number}` ---A range between 0 and 1.

Returns
:    1. `{boolean}` ---Returns `true` on success, `false` if not.

Sets the volume of the sound, with 1 as the maximum, and 0
as silent. A sound's default volume is 1.

~~~
audiomanager.setVolume('levelmusic', 0.8);
~~~

### getVolume (name)

Parameters
:    1. `name {string}`

Returns
:    1. `{number}`

Returns the volume level of the specified sound.

~~~
audiomanager.getVolume('levelmusic'); //=> 0.8
~~~

### setMuted (isMuted)

Parameters
:    1. `isMuted {boolean}`

Mute volume on all sounds.

### getMuted ()

Returns
:    1. `{boolean}`

Check the mute status of all sounds.

### setMusicMuted (isMuted)

Parameters
:    1. `isMuted {boolean}`

Mute volume on music sound.

### getMusicMuted ()

Returns
:    1. `{boolean}`

Check the mute status of the music sound.

### setEffectsMuted (areEffectsMuted)

Parameters
:    1. `areEffectsMuted {}`

Mute volume on effects sounds.

### getEffectsMuted ()

Returns
:    1. `{boolean}`

Check the mute status of the effects sounds.

### setTime (name, time)

Parameters
:    1. `name {string}`
     2. `time {number}`

Sets the current time of the specified sound. Note that
this function is only available for `background` sounds.

### getTime (name)

Parameters
:    1. `name {string}`

Returns
:    1. `{number}`

Returns the current time of the specified sound. Note that
this function is only available for `background` sounds.

### getDuration (name)

Parameters
:    1. `name {string}`

Returns
:    1. `{number}`

Returns the duration of the specified sound. Note that
this function is only available for `background` sounds.

### setPath (path)

Parameters
:    1. `path {string}`

Set a new path for the sound group.

~~~
audiomanager.setPath('resources/sounds/music');
~~~

### getPath ([name])

Parameters
:    1. `name {string}` ---Optional sound name.

Returns
:    2. `{string}`

~~~
audiomanager.getPath();        //=> "resources/sounds"
audiomanager.getPath('boink'); //=> "resources/sounds/effect"
~~~

### getExt ()

Returns
:    1. `{string}`

Returns the file extension of the sound.

~~~
audiomanager.getExt() //=> ".mp3"
~~~
