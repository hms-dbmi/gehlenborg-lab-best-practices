# gehlenborg-lab-best-practices
Guidelines for creating medium-scale visualization software

## Find a problem

- If you’re not in the field, is there at least one person who will give you feedback about whether what you’re doing makes sense?
- … and if you are the expert, it’s still important to check in with peers for reality checks.
- How will you know if your solution is a good one?

## Mockup before coding

- It’s easier to draw than to code, and it’s easier with pencil on paper than on a screen.
- When you show it to a real person, can they find the information they need, and are the interactions what they expect?
- Once you’ve started coding, editing the HTML in the browser debugger can sometimes give you a quick idea of what a tweak in the code will look like.
- Illustrator / Swagger

## Choose a framework

- Do you want to learn a new framework, or is a tool that’s already in your kit a better choice? Learning is good, but also think about your timelines, the maturity of the new tool, and what the community around it looks like.
- (Lean towards what we’re already familiar with.)
- If you’re leaning towards tool X, because only X works with platform Y, that might be a warning. Does the platform dictate a particular tool choice rather than offering an API you can satisfy?
- On the other hand, creating a plugin for a widely adopted platform Y can also be a good choice: It may have standards around packaging and documentation that will keep you from reinventing the wheel, and there may be an active user community you can turn to.

## Use version control

- As soon as you start coding, use version control. Github has worked for us: I typically start a project in my own space, and when it looks like something that’s not just scratch I’ll transfer it to [hms-dbmi](https://github.com/hms-dbmi) or [refinery-platform](https://github.com/refinery-platform/). (Links to your old personal repo will redirect so don’t worry about dead links.)
- Default to open and public; lean towards hms-dbmi.
- Make sure you fill in the basics: Description (shows up in project listings), README, LICENSE (Default to MIT), and add a tag for gehlenborglab.
- Once you’re a little ways in, it’s good to start working in branches:
  - You can make sure each incremental change is backed up at github, without getting confused about what the “latest version” is.
  - You can get get feedback from colleagues on PRs.
  - You can switch between work on different features. 

## Document

- *Code comments are always good: “Why” and maybe “What”*
- At a minimum, there should be a README.md that explains the project:
  - What does it do? Who would use it?
  - What precisely does someone need to type to see it run?
  - If it needs sample data to work, where can they get it? (or just check it in! Or S3)
  - Screen shot? (but maybe avoid animations.)
  - Quote the command line help, or online docs?
  - Where does funding come from?
- You might have multiple .md files if you want to provide more background. (Prefer markdown in repo to wiki or outside docs?)
- … but we haven’t worried about pydoc or other generated documentation
  - But… docstrings can be invaluable, especially in weakly typed languages like Javascript and python
  - Docstrings should summarize exactly what the function does and list the parameters and their types

## Test

- At a minimum, write unit tests as you code, hook up TravisCI, and don’t merge until the tests pass.
- Manual testing probably has a role.
- Think about whether higher level tests are appropriate. At least one test that checks for a 200 response is good, and figuring out selenium or cypress may also be useful, depending on the project.
- Code coverage tooling
- … and think about what other assertions you’d like to make on your project: Travis can run any command line tool, so you might think about style checkers, test coverage, link checking…
- And when you’ve done all that, put it all in one script, so you can do the same thing locally that Travis does.

## Version and package and reuse

There’s often some conventional artifact that should be the outcome of a project (Docker containers, Python or R packages, ...) and there’s usually a conventional host for these artifacts (DockerHub, PyPi, CRAN, …)

For some of these, Travis offers an integration out of the box… for the more idiosyncratic ones, you’ll need to tell Travis what to do.

You also don’t want to release after every commit: You’ll probably git tag releases, and when you push the tag, that will trigger the deployment process in Travis. We can point you at more examples. As you tag, semantic versioning is probably a good model, but we do not have a clear sense of when it’s time to move past v0.0.X.

## Granular repos

Keep repos small. You message will be more clear, the build process will be simpler, the tests will run faster, and you’ll accumulate less tech debt, and you’ll make code reuse easier.

When a project is composed of multiple tools, use the dependency manager appropriate for your system, and reference tagged releases.

But… you’ll need to keep these versions up to date: suppliers may drop older ones.

Semantic versioning

## Track issues

Use an issue tracker: Github is probably the default, but if you have good reason to use something else, don’t feel constrained.

- Make sure users can submit bugs.
- Template
- When a bug crops up try to capture as much information as possible so you can reproduce: What version are you running? What browser? What data? Consider using github issue templates.
- …. On the other hand, don’t over-specify new features when you first file them. The story should be clear, but don’t paint yourself into a corner.

## Automate

If you’re doing something by hand that the computer could do, figure out how to make that happen. Maybe there’s a tool already out there, or make it takes a bash one-liner.

## Errors

- Never silence errors! 
- Statements like this can lead to difficult to find bugs:
```python
try:
    doSomething()
except Exception as ex:
    pass
```
- Either handle the error gracefully or print a warning that something unexpected happened
- Example (Pete): “I was looking at an old example and the data wasn’t loading in the browser. The console didn’t show any issues. There was an incomplete request but I didn’t pay it much mind because we often request things that can fail. Going through the code and adding log statements led me to a chunk of code containing an if statement that deliberately ignored the error. To fix this, I added an error callback that got called with the error text.
```javascript
if (error) {

} else {
    callback(data)
}
```

## Ask questions

Slack is often a good medium, as is chatting over lunch. Ask for PR reviews, or find the time to do larger code reviews with someone in the lab.

## DON’T WORRY!!!
