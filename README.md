# Markov Junior Notes


[This](https://github.com/mxgmn/MarkovJunior) is incredible but extremely dense and not packaged in a super user-friendly way. So these are my notes.

## How do I run this?

It's using .NET core. So theoretically you can use `brew` or `chocolatey`, except it looks like it's built using .NET core 6, which is the latest and I think chocolatey hasn't updated yet. Also on OSX you need to add an extra dependency to get System.Draw to work properly.

There's a built executable for windows but you still need the dotnet runtime anyway to run the exe.

## What is happening when I run the exe?

It's running through a large set of models and generating png files in the output folder. There is no .gitignore file, so your modified files are gonna blow up

## How do I control which models get executed?

There is a `models.xml` file that controls what to run and what settings to run it with.

## How do I get the fancy animated gifs or better yet an mp4?

If you run a model with `gif="True"`, then it generates a png file per step. ffmpeg can convert those images into a video. VLC can play videos produced by ffpmpeg (Quicktime frequently cannot). If you put it in gif mode the application refuses to go more than 1000 steps.

So the fast command to generate a video from a model looks something like this:
`MarkovJunior.exe && ffmpeg -i output/%d.png out.mp4 && vlc out.mp4`

The `%d` in the ffmpeg command processes the filenames in numeric order instead of lexical order. A shell expansion can choose to do it in lexical order and you'll have some of the frames in a weird order.

## What are all of the options in `models.xml`?

* __size__: linear size. Seems to be a way to set both the width and height of the image in a single command
* __d__: dimension. defaults to 2. I've only tested 2 so far. It would be neat to see what 1 or 4 does.
* __length__ : number
* __width__: number
* __height__: Probably only necessary for 3D
* __amount__: defaults to 2. The number of seeds to run.
* __pixel_size__: defaults to 4. Makes it easier to see at default zoom size.
* __seeds__: space separated numbers
* __gif__: boolean, gif mode has slightly different behavior the most important is probably that it generates a png on every step instead of only generating the final product
* __iso__: boolean
* __steps__: the number of steps to run for. If it's unspecified, it defaults to 1000 for gif and 50000 for non-gif mode
* __gui__: a number, the width of the rule tree data. Some rule sets are wider and need more space

## How does the model language work?

Remarkably similar to behavior trees

Consists of a single root-level node and child nodes.

### Composite Nodes
* __markov__: Execute the first child that has a match
*  __sequence__: Execute the first child that matches, but don't run children that have already completed
*  __all__: Every rule should find all matches and do the replacement, if there are multiple matches for the same position, then it can use `field`s to bias the result

### Action Nodes
* __one__: find a single match and do the replacement. if steps is defined, it will terminate the action
* __all__: find all matches and do the replacement.
* __prl__:
* 

