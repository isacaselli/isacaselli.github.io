---
title: "Contribution to GNOME Files: bitrate information display in audio files"
date: 2025-05-14 14:00:00 -0300
categories: [Free Software, Contributions]
tags: [kernel, contributions, GNOME, Git, KW]
comments: false
---

In a previous post, we talked about searching for issue in a few GNOME systems, and that a issue from GNOME Files caught our attention. Let's take a better look to the issue and how we tried to solve it.

## The problem: bit rate not showing up 

Looking through the code in that file, we noticed it was already trying to fetch the bit rate using the gst_discoverer_audio_info_get_bitrate method. If the method was already in use, why wasn’t it working?
That's when Rodrigo decided to comment on the issue to get clarification. A different maintainer replied and confirmed that, yes, the bit rate was being queried, but the value returned was 0, and because of that, the bitrate information was ignored:

```c
static void
set_bitrate (TotemPropertiesView *props,
             guint                bitrate,
             const char          *title)
{
    char *string;

    if (!bitrate)
    {
        return;
    }
    string = g_strdup_printf (_("%d kbps"), bitrate / 1000);
    append_item (props, title, string);
    g_free (string);
}
```

## How we tried to solve it

Still, we weren’t ready to give up. GStreamer was successfully displaying lots of other metadata, so it didn’t make sense for just the bit rate to be missing. After some reading, we discovered that .FLAC files actually don’t store bit rate as part of their metadata — which explained the 0 value we were getting. So, if the bit rate isn’t available directly, should we just drop the idea? Not quite. We figured we could calculate it manually. Our first attempt used a theoretical formula:

    bit rate = sample size × number of samples × number of channels.

After adding this calculation to the code, we started seeing a bit rate value being displayed for our test files. However, there was a catch — this formula doesn’t account for compression, which .FLAC files use. That meant our result was too high and didn’t reflect the actual bit rate.

Looking for a better alternative, we found another approach:   

    bit rate = file size (in bytes) × 8 / duration (in seconds).

## Implementation

With this new formula, our goal was to access both the file size and its duration. We couldn’t do this inside the update_audio function alone, so we modified it to accept two new parameters. The function discovered_cb, which calls update_audio, already had access to the file URI and duration, so we passed them along:

```c
GFile *file = g_file_new_for_uri (gst_discoverer_info_get_uri (info));

if (audio_streams)
{
    update_audio (props, audio_streams->data, file, duration);
}
```
Now, it's possible to get the information needed from audio file and the file size:

```c
guint64 duration_seconds;
goffset filesize;
GFileInfo *fileinfo;

fileinfo = g_file_query_info (file, G_FILE_ATTRIBUTE_STANDARD_SIZE, G_FILE_QUERY_INFO_NONE, NULL, NULL);
if (fileinfo) 
{
    filesize = g_file_info_get_size (fileinfo);
    duration_seconds = duration / GST_SECOND;

    g_object_unref (fileinfo);
}
else 
{
    duration_seconds = 0;
}
g_object_unref (file);
```

Inside update_audio, we queried the file size and used the existing duration to compute the bit rate:

```c

if (filesize)
{
    bitrate = filesize * 8 / duration_seconds;
}
else 
{
    bitrate = gst_discoverer_audio_info_get_bitrate (info);
}

set_bitrate (props, bitrate, _("Audio Bit Rate"));
```
## Viewing the result

| Audio properties without bit rate | Audio properties with bit rate displayed |
|:---------------------------------:|:----------------------------------------:|
| ![](/assets/img/audio-without-bitrate.png) | ![](/assets/img/audio-with-bitrate.png) |
| *No bit rate shown in the properties window* | *Bit rate (950 kbps) successfully displayed* |


To test if it was all working, we used a sample .FLAC file and compared our result with the value shown by an online bit rate calculator. The result from our implementation was very close to the expected value — around 943 kbps in our test. The slight difference might be due to extra factors the online tool takes into account, but our method still produced a very accurate estimate. We tried other files too, and the values were consistently close.

So, we’ve managed to solve the original issue — at least for audio files, as reported. Now we’re waiting for feedback from the GNOME contributors on our final comment, and then we’ll be ready to submit our first merge request!

Fingers crossed that I’ll be back here soon with some good news about it being accepted.