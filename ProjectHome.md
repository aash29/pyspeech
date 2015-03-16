# I am no longer maintaining this library.  It may work for you, but if not, try out the [Dragonfly](http://code.google.com/p/dragonfly/) project instead. #



&lt;hr&gt;



# speech.py #

speech.py is a Python module that provides a clean interface to Windows's voice recognition and text-to-speech capabilities.  It's very easy to use within a program that needs to listen for specific phrases or general speech, or that needs to speak.

It is available on PyPI at http://pypi.python.org/pypi/speech/ .


## Example code ##

Here's a very simple program that repeats whatever it hears you say, until you say "turn off".

```
import speech

while True:
    phrase = speech.input()
    speech.say("You said %s" % phrase)
    if phrase == "turn off":
        break
```

`speech.input()` in the above example blocks the program until it hears you.  Below is the same example using non-blocking listeners, so that your program can do other things while waiting for input.

```
import speech

def response(phrase, listener):
    speech.say("You said %s" % phrase)
    if phrase == "turn off":
        listener.stoplistening()

listener = speech.listenforanything(response)

# Your program can do whatever it wants now, and when a spoken phrase is heard,
# response() will be called on a separate thread.
import time
while listener.islistening():
    time.sleep(1)
    print "Still waiting..."
```

More complex examples are available [here](Examples.md).


## Requirements: ##

Windows XP or Vista, and Python 2.4 or 2.5.  If you use Windows Vista, you'll need to say "start listening" if Speech Recognition is not awake.


## Installation: ##

Type `easy_install speech` at the Windows command prompt.  (If that doesn't work, download and run [this Python script](http://peak.telecommunity.com/dist/ez_setup.py) to install easy\_install, then try again from C:\<Your python directory>\Scripts\ .)

If you don't already have it, you'll also need pywin32 ([for Python 2.5](http://tinyurl.com/5ezco9) or [for Python 2.4](http://tinyurl.com/5uzpox)); and if you're on XP, you'll need the Microsoft Speech kit (installer [here](http://tinyurl.com/br8ysh)).

## See Also: ##

You may also find Christo Butcher's [Dragonfly](http://dragonfly.googlecode.com) package useful.  Dragonfly is a Python-based speech recognition framework which lets you easily implement complex voice commands, including dynamic elements and free-form dictation.

## Thanks ##

To Inigo Surguy for his original code which helped me figure out COM and the Microsoft Speech SDK, and to Dekudude for suggesting the need for `speech.input()`.