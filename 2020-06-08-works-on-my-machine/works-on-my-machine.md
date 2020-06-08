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

## Trial and Error

When you're in this kind of situation, you need to have a picture of what's happening. First, you just tries random stuff hat should help sometimes like "Clean build", ask collegue, ask Rubber duck, ask StackOverflow. If nothing helps, you need just facts. The more facts you have - the better. While there can be gazillion of possible reasons of the bug, facts are narrowing down the scope of the search.

These were mine:
- Custom NSTextField subclass used in Framework
- Seems to be working out of the main project just fine
- Works the same in Release/Debug builds* (Doesn't work, I mean)
- ~Debugger~ NSLog shows that textField is actually multilined only if debugger attached

And then, I finally found it.

## To category or not to category. That is the question.

All you know that categories are ~good~ ~evil~ \[ insert your opinion here\]. Sometimes, categories are helful, sometimes, they're harmful. While many of you probably have read lines below, not many of you can answer on a question what will happen if method in cathegory will collide with a method of the original class.

> If the name of a method declared in a category is the same as a method in the original class, or a method in another category on the same class (or even a superclass), the behavior is undefined as to which method implementation is used at runtime.
https://developer.apple.com/library/archive/qa/qa1908/_index.html

If you ask a developer what could happen if methods will collide, probably the most frequent answer will be "crash" \[ checking \]






