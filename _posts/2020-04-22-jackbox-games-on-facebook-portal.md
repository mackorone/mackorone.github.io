---
tags: [software]
---
Disclaimer: I work at Facebook, but not on Portal.

Facebook Portal is great for keeping in touch with distant relatives.
Having a dedicated screen for video calls makes all the difference.

Recently, my family got the itch to play a game - any game - over video chat. I
suggested we try [Jackbox Games](https://www.jackboxgames.com/) since they're
basically designed for remote play. Later I realized that the multiple
screens required to participate might pose a barrier to some players, my
grandparents in particular. By default, we'd each need:

1. A screen for the game itself (e.g., television)
1. A screen per controller (e.g., smartphone)
1. A screen for video chat (e.g., Portal)

Thankfully, with some fiddling, I figured out how to share the game screen to
the Portal (Messenger) call, reducing the number of screens required from three
to two. Portal prevents screen sharing - I have no idea why - so I had
to trick Messenger into thinking the game running on my desktop was a webcam,
and then join the call. Here's how I did it:

### Virtual Webcam

I used [OBS](https://obsproject.com/) to compose a scene
consisting solely of the game. Then I downloaded
[OBS-VirtualCam](https://github.com/CatxFish/obs-virtual-cam), a tool that makes
OBS output available as a virtual webcam. After enabling the tool by clicking
the "Start" button, the virtual webcam was recognized by Chrome. For some
reason, the Messenger web client refused to allow me to choose the virtual
camera (the dropdown was disabled), but unplugging my physical webcam was enough
to get Messenger to default to the virtual one. Within only a few minutes, I
could call my desktop from my Portal, using separate Messenger accounts, and
display the game screen on the device.

Unfortunately, I couldn't get the audio
to work. Turns out, OBS-VirtualCam explicitly doesn't support it yet. The
reason?
> ["Microsoft request a paid
certificate to create a virtual sound
card."](https://github.com/CatxFish/obs-virtual-cam/issues/38) -CatxFish

### Virtual Microphone

Since OBS doesn't solve the audio problem, I needed to find something that
does. After a bit of searching I stumbled upon a
[few](https://obsproject.com/forum/threads/obs-as-an-audio-input-virtualcam-but-for-audio.116802/)
[threads](https://obsproject.com/forum/threads/desktop-audio-from-virtual-cam.83763/)
mentioning [VB-Audio Voicemeeter Banana](https://www.vb-audio.com/Voicemeeter/banana.htm),
a virtual audio mixer, and decided to give it a go. It's like OBS, but for
audio. I watched a [YouTube video](https://www.youtube.com/watch?v=8c1LPeyVjdE)
to learn the basics and quickly figured out a simple setup that requires nearly
zero configuration:
- Point the game audio at Voicemeeter Banana Virtual Input (advanced
  sound settings)
- Set the Chrome microphone to Voicemeeter Banana Virtual Output (from the
  dropdown)

One note: I had to select a main output device - the flashing prompt in the
upper right corner - before sound actually came through on the Portal. I
chose my headphones but I don't think it matters.

### Final Result

{% include video.html
  alt="Drawful game on Facebook Portal"
  src="/assets/images/jackbox.mov"
%}
