**How do I make speech recognition not suck?**

Microsoft's Speech Recognition is awful at first, especially when not listening for a short list of phrases.  Be sure to train it: Start Menu -> Settings -> Control Panel -> Speech -> Train Profile.

**Does it work on Linux or Mac?**

Nope.  Unfortunately, Linux doesn't have any speech recognition support worth talking about yet.  And I don't have a Mac so I can't figure out if it could be made to work there.  If anyone does have a Mac and is willing to work with me on getting Mac support built in, I'd be more than happy to help!

**Why shouldn't I use PyTTS instead?**

The [PyTTS](http://www.cs.unc.edu/Research/assist/doc/pytts/) package provides text-to-speech service, not speech recognition service.  However, it gives you much more control over the voice (pitch, speed, etc) -- speech.py just uses the default Windows voice as a convenience in case you need something spoken.  If you need speech recognition and also precise control of the computer's voice, use both packages.

**I'm on Vista, and I get an error when I use speech.say().**

If you have installed SAPI5VoiceInstaller.msi on Windows Vista, you didn't need to and it breaks Windows's voice synthesis.  Just select Anna as the default voice in the Control Panel and things should work again.