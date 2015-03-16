If you have any trouble with the examples below, please [leave a comment](Discussion.md) about it, and I or another user will be glad to help you out.

## Quick reference ##

`speech` module functions:

  * `speech.say(phrase)`: speak out loud.
  * `speech.input(prompt=None, phraselist=None)`: print the prompt, then return the phrase heard from the phraselist.  Blocks the thread.

  * `speech.listenforanything(callback) -> Listener`: when any dictation is heard, run the callback on a separate thread (see below for details).  Doesn't block the thread.
  * `speech.listenfor(phraselist, callback) -> Listener`: when any string in the given list of phrases is heard, run the callback on a separate thread (see below for details.)  Doesn't block the thread.

  * `speech.islistening()`: True if any `Listener`s are listening.
  * `speech.stoplistening()`: stops all `Listener`s.

Format of the callback function passed to `listenfor()` and `listenforanything()`:
  * `callback(heard_phrase, Listener)`: First arg is a string containing the text of the phrase that was heard.  Second arg is the `Listener` that heard the phrase -- the same `Listener` that was returned by `listenfor()` in the first place.

`Listener` instance methods (call these on the object returned by `listenfor()` or passed to the callback):

  * `islistening()`: True if the `Listener` hasn't been stopped yet.
  * `stoplistening()`: stops the `Listener`.  You can't start it again after that.

## Simplest example ##

Whenever you say the name of a dwarf, it says it back, until you say "turn off".  Your program can't do anything else while it's waiting for speech.

```
import speech

while True:
    phrase = speech.input("Say the name of a dwarf.",
       ['Sleepy', 'Happy', 'Dopey', 'Killer', 'Sneezy', 'Bashful', 'Doc', 'turn off'] )
    speech.say("You said %s" % phrase)
    if phrase == "turn off":
        break
```

Note that `speech.input()` will only return when you say something in the given phraselist.  If you don't pass in a phraselist, `speech.input()` will accept anything at all.

## Simple example, non-blocking ##

`speech.input()` in the above example blocks the program until it hears you.  Below is the same example using non-blocking listeners, so that your program can do other things while waiting for speech input.

```
import speech

def response(phrase, listener):
    speech.say("You said %s" % phrase)
    if phrase == "turn off":
        listener.stoplistening()

listener = speech.listenfor(
    ['Sleepy', 'Happy', 'Dopey', 'Killer', 'Sneezy', 'Bashful', 'Doc', 'turn off'],
    response)

# Your program can do whatever it wants now, and when a dwarf's name is heard,
# response() will be called on a separate thread.  This can happen over and over --
# as long as the listener hasn't been stopped.
import time
while listener.islistening():
    time.sleep(1)
    print "Still waiting..."
```

The example above uses 8 phrases, but you can use as many phrases as you wish; I've successfully used 12,000 sentences!

## Using listenforanything() ##

Just as `speech.input()` can leave out the phraselist to accept general dictation, if you replace the `listenfor` call above with

` listener = speech.listenforanything(response) `

then `speech` will call the response callback as soon as any recognizable speech is heard.

## Using all functionality ##

See the inline comments and assertions.

```
import speech

response = ''
while response.lower() != 'uncle':
  speech.say("Say uncle!")
  response = speech.input()

# Handle a specific set of heard phrases with a callback.
def L1callback(phrase, listener):
  print "Heard the phrase: %s" % phrase
L1 = speech.listenfor(["any of", "these will", "match"], L1callback)
       
# You can listen for multiple things at once, doing different
# things for each.
def L2callback(phrase, listener):
  print "Another phrase: %s" % phrase
L2 = speech.listenfor(["good morning Michael"], L2callback)

# You can listen for general dictation as well.  You can
# stop listening using the callback's second argument.
def L3callback(phrase, listener):
  speech.say(phrase) # repeat it back
  if phrase == "stop now please":
    listener.stoplistening()

L3 = speech.listenforanything(L3callback)

# All callbacks get automatically executed on a single separate thread.
# Meanwhile, you can just do whatever with your program, or sleep.
# As long as your main program is running code, speech will keep
# listening.

import time
while L3.islistening(): # till "stop now please" is heard
  time.sleep(1)

assert not L3.islistening()
assert speech.islistening() # to anything
print "Dictation is now stopped, L1 and L2 are still going."

L1.stoplistening()
print "Now only L2 is going"

# Listen for L2 for a while more, then turn it off.
time.sleep(30)

speech.stoplistening() # stop all remaining listeners
assert not speech.islistening()

```