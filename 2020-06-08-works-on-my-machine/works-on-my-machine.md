# Works on my machine

You know that situation, when you have done everything right. You checked everything. And still, there's a bug that feature X is not working. Sometimes, you really missed something, sometimes there's a some level of weirdness going on. The fun thing that the more experience you have, the less magical it looks. It's just because you've been in that situation and know potential reasons causing incorrect behaviour.

I'm pretty sure that most of the developers have experienced or will experience these situations:

- The code works in Debug only
- The code works in Release only
- The code works correctly on some versions of the operating system,
- The code works correctly only if compiled with specific SDK
- Changing the source doesn't affect application

Here's a harder one.

- The code works correctly only with attached debugger

## Double slit experiment

There's an [experiment](https://en.wikipedia.org/wiki/Double-slit_experiment) in physics that shows that results of the experiments will differ depending on observation status. This is the closest example of what I've experienced when faced te bug.

There was a nasty layout bug in application, with one multilined field, that was rendering itself as multiline with debugger attached. Without the debugger that always rendered as single-lined.

![image](https://user-images.githubusercontent.com/119268/84046935-63fd3480-a9b3-11ea-9733-00e9487cdf8f.png)


