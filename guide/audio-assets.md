# Creating Audio Assets

This guide will get you started creating and managing audio
assets for your game. To information about how to *use*
sound within your game, consult the [AudioManager API](../api/audio.html).

## The Essentials

On iOS you cannot play .OGG audio, so ensure that your sound files are in .MP3 format as discussed in this guide.  With the Game Closure DevKit, you can easily support both .OGG for Android and .MP3 for iOS by having two copies of each sound file.  When building, devkit will take the .OGG version when it can be used on the target platform (Android for example) and will fall back to the .MP3 extension version if needed (iOS).  Inside your game, name your sound files with a .MP3 extension rather than .OGG.

## Audio Recording and Editing Tools

There are a wide variety of hardware and software solutions
available for recording and editing audio for games. If
you're planning on creating the audio assets yourself, you
may already have a preferred program for editing digital
audio. However, if you're new to digital audio recording and
editing and want to gain a better understanding of how it all works,
[Audacity](http://audacity.sourceforge.net) is an open
source, free digital audio workstation (DAW), that can
handle many of your audio needs.

## Audio File Size and Format

Whether you're creating the assets yourself, or having them
delivered by a third party, the most important aspect of any
audio asset, for developers, will be file size and format.

### Format

The Game Closure DevKit does not place restrictions on any audio
formats, but it is important for developers to be aware of the
format restrictions of the operating systems, and browsers,
on which their game will be deployed. Check the target
platform's website for the latest information on supported
audio file formats, [the browser](https://developer.mozilla.org/en-US/docs/HTML/Supported_media_formats#Browser_compatibility), [iOS](http://developer.apple.com/library/ios/#documentation/AudioVideo/Conceptual/MultimediaPG/UsingAudio/UsingAudio.html) and [Android](http://developer.android.com/guide/appendix/media-formats.html).

Other important considerations, when choosing a file format
for your audio assets, are restrictions that are inherent to
the format. For example, while MP3 is often a good choice
for games because of its small file size and almost universal
compatibility, it is generally not an ideal format for
looping audio. This is because the MP3 format adds brief
amounts of silence to the beginning and end of any given
audio file. This addition of silence is inherent to the
MP3 format, and although the added silence is brief, it is
generally noticeable to listeners as a gap or pop in the
audio. For this reason, it is often better practice to use
an alternate format for an audio asset that will play on a
loop.

### File Size

The file size of a given asset is generally an important
issue in game development, since it can affect the overall
performance of the game. Audio assets often require a
relatively large amount of disk space when compared to other
assets, such as images. For this reason, it is important to
choose a format, and compression rate (when applicable),
that reduces the size of the audio file, while preserving as
much of the audio quality as possible.

The first step is finding a reliable audio converter that
can compress the audio to the desired format and file size.
There can be a wide variance in the quality of audio
converters, particularly when converting at low bit rates,
such as 112 kbps, or 64kbps. Professional DAWs will often
produce significantly higher quality conversions than most
stand-alone, or batch audio converters. However, it is
important to make sure that a given DAW can convert to the
desired format, as formatting options within DAWs can vary
widely.

There is no one conversion rate that suits all audio, and
you may find that some sounds have reasonable fidelity at
extremely low conversion rates, whereas others require
something significantly higher. It's generally best to
consider audio files on an individual basis, and convert
them at the lowest rate that preserves the desired
fidelity.

#### Stereo and Mono Files

Another important consideration for audio content is whether
the audio needs to be in stereo or mono. This is
significant because it can drastically affect the size of
the audio file. In uncompressed formats, such as WAV or
AIFF, stereo files require twice the amount of disk space
that mono files require. While this is not the case for all
compressed formats, it is generally best practice to use
mono files when possible, and stereo files only when
necessary.

Another factor to consider when choosing between stereo and
mono files is the speaker configuration of the target
platform. For example, most mobile devices only have one
speaker. This means that even your stereo files will be
effectively summed to mono when a user is listening to the
audio on the device. However, if the user plugs-in
headphones, stereo files will be heard as stereo, and mono
files will be heard as mono.

For these reasons, among others, many developers will only
make their music files stereo, while keeping all sound
effects in mono (perhaps with the exception of particularly
special sound effects).

#### Sample Rates

All digital audio is re-coded at a particular sample rate.
That sample rate also drastically affects file size, and it
is important for developers to be aware of any limitations
that a particular target platform may place on sample
rates.

The sample rate for CD quality audio is 44.1kHz. Most
target platforms will support this, as well as any lower
sample rates. 44.1kHz is also the rate to which most DAWs
will default. Higher sample rates will generally not be
used in game development, since higher sample rates require
larger amounts of disk space. However, since lower sample
rates use less disk space, it is often worth while to check
the quality of a given audio asset when it is converted to a
lower sample rate, and use it if the quality is suitable.

## Tips for Audio Production

Whether you are creating the audio content yourself, or
having it produced by a third party, the following tips may
be helpful during the development process:

* Decide what audio content will need to loop, and choose
  your audio file formats accordingly (remember that some
  formats loop better than others).
* Always check your audio content in mono, since many users
  may only hear the audio from mobile devices that have only
  one speaker.
* Check your audio on the target platform's speakers (when
  possible). If you're developing a game for iOS, listen to
  the audio content on an iPod, iPad, and/or iPhone. Audio
  content often sounds significantly different on a device
  speaker, due to its physical limitations, and you'll want
  to make sure your audio sounds great to the end user
  regardless of how they choose to listen to it.
