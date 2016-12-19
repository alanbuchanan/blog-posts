# 4 awesome Git features that will make your life easier

Using git, us developers can easily shift into the habit of repeating the same `git` commands over and over. All we need is to be able to push and pull, right?
Wrong!

There's a wealth of awesome features available to us in git. They make our development experience easier and more enjoyable, and we are likely to be saving time with every extra feature we add to our git armoury. Here are a few cool git features you might not know about:

1. A nicely formatted commit history right there in the terminal

You're working on a project with many devs who commit regularly. You have a rich commit history because of this. How can we view this history in a pleasing way?

This is where `git log` comes in. It exists for displaying history of your git project. Using flags, we can define how much or how little of our history we want to see, right in the terminal.

As you'll see in the documentation, `git log` has dozens and dozens of options we can pass it.
https://git-scm.com/docs/git-log

I find the standard `git log` output to be not particularly useful or nice to look at. I want more useful information in a more compact view. As an example here's what I use for my log whenever I want to see my history:
```
git log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)'
```
This shows us a pretty log of the history on the project. This gives us all we need to know about what was committed and when, what status our HEAD is in, how many commits were made today, and much more.



2. Stage a file by its index shown in `status`

When we have changes in our working directory, and we use `git status` to see all our changed files, what happens when we know exactly which file to add for staging, but it's a pain to reference?

```
home/users/bert/stuff/code/my-project/index.html
home/users/bert/stuff/code/my-project/src/main.js
home/users/bert/stuff/code/your-project/styles/normalize.css
home/users/bert/other-folder/another-long-pathname/bert.js
```

It might have a super long and specific path that, frankly, we'd rather not spend a good 30 seconds typing out - even taking into consideration fuzzy find!

We just want that one file - say, the 3rd one down, `home/users/bert/stuff/code/your-project/styles/normalize.css`.

There's a great solution for this - let's open our `.bashrc` or `.zshrc` and create a function for it:
```
git-add-n () {
    git status --short \
        | sed -n "$1"p \
        | awk '{ print  $2 }' \
        | xargs git add;
}
```

What this is going to do is run `git status --short`, which will run a short version of `git status`, then grab the line of the number we pass to it, and run `git add` with the filename.

For example, we want the third file down in our list, `normalize.css`, so we run `git-add-n 3`. The third item in the list is now added.

Imagine if you wanted to make 10 separate commits with 10 separate files. The amount of time you save by incorporating this into your workflow is worth it!



3. Stage in chunks, not all at once!

Another common issue I've encountered with staging is that you might have many different changes across files, but you don't want to commit them all at once.

This is where `git add --patch` comes in useful. Instead of adding all changes, or changes by file, we stage in chunks.

When running `git add --patch`, a prompt will appear asking us if we want to stage a particular chunk of our working copy. If we say 'yes', the chunk is staged. If we say 'no', it's not, and it remains in our working copy.

Staging in this way means that our commits are more compact and more useful to anyone else working on the project. All of our features, even though they were in the same working copy, are split out into separate commits, because we used `git add --patch`.

There are many other features to `git add --patch` if you really want to get advanced with your staging.

There's also `git checkout --patch` which essentially does the opposite - removing chunks from your working copy instead of staging them. Have a few features you want to stage and a few you want to scrap? Run `git add --patch` to stage the useful stuff, then ditch the dirt with `git checkout --patch` straight after!

The other important thing to consider with these approaches is that we'll be more sure about exactly what we're committing before we commit it, because we see the diff right there pre-commit. We're more likely to see testing code that we accidentally left in.



4. Find where that pesky bug was introduced to the project

If you're working on a big project with many other developers, it can be really hard to spot where bugs came from.

Was it from today or yesterday or neither? Was that added as part of a particular ticket by mistake? Who could've added this and why could it have been added? It can be overwhelming to think about how something got into the project, but bugs happen, so how do we find how they got there? We can't get our way to how the bug got there - it's just too painful.

The secret lies in `git bisect`. Using this, we can detect exactly which commit introduced the bug. After we give `git bisect` a 'good' commit ID - where the bug isn't present, and a 'bad' commit ID - where the bug IS present, it will automatically checkout to the middle commit between these two points. Now we can tell it whether this commit is 'good' or 'bad', and the process repeats until we have narrowed it down to one commit - the one remaining 'bad' commit.

So by using `git bisect`, we've saved potentially hours of frustration. We just tell it if what we see is good or bad, and it sniffs out the offending commit for us!

https://git-scm.com/docs/git-bisect


