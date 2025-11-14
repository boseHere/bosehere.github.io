---
layout: post
title: Everything I know about Singularity
date: 2025-11-10
description: My singularity presentation
tags: singularity tutorial
categories: presentation
featured: true
---


# Everything I know about Singularity
Adapted from my lab presentation "Everything I know about Singularity in 31 slides".

## What is Singularity?

> Insert slide 2 photo

You can think of a Singularity "container" as a tiny computer you can save as a file and execute software from. 

On a more technical level, a Singularity container is the smallest amount of operating system and software you need to run some piece of code. 

When you run a Python script on your computer, you are relying on:
1. The libraries you are importing to use in your code (pytorch, seaborn, etc.)
2. An installation of Python to interpret your code
3. An operating system (be that Linux, Windows, Mac, etc.) for Python to be installed on.

A Singularity container that can run your code would have those 3 things and nothing more. 

## Why should I use Singularity?

Singularity makes it easy to handle the competing version needs of different pieces of software. 
> Insert slide 3 photo

Additionally, Singularity makes it **much** easier to share your computing environment with other people

For example, if Joseph made a singularity container with Cicero installed on it, and Christa needed to run Cicero, Joseph could just send Christa his container! Christa could even install more packages on top of this as they needed (Note: this is not allowed on some HPC servers).

You could use Conda, but:
1. Conda package management just breaks sometimes. Once you have a singularity container, it'll work forever. 
2. Sharing a conda environment requires exporting a package list and then rebuilding that environment from scratch. With singularity the container file can be shared directly or pulled from a repository.
3. 
If you are currently using conda, you can also convert your conda environments into Singularity containers using my tutorial at the end of this presentation. 

## How do I make a Singularity container?

### The Build file
The "Build file" is the essential component of making Singularity containers.

It is standard to name this file `Singularity`.

You can think of this like the `Snakefile` in snakemake, which defines how to create files through a workflow, or a `Makefile` in C, which defines how to compile program files. 

A build file can be broken down into 2 main components:
1. Define the container that you're building on top of (required)
2. Install stuff on that (optional)

What does this look like in practice?

#### Full example: building from Docker

Let's look at an example of a build file:


```singularity
Bootstrap:docker
From:bioconductor/bioconductor:RELEASE_3_21-R-4.5.0

%post
apt-get update && apt-get upgrade -y

R -e "BiocManager::install('ComplexHeatmap')"
R -e "install.packages('tidyverse', dependencies=TRUE, repos='https://cloud.r-project.org')"
```

That's a lot of words. 

#### Other Bootstrap options

## How do I run code with a Singularity container?

### Run code from a Singularity container on the command line

### Run code from a Singularity container with Snakemake

### Run a jupyter notebook/lab from a Singularity container


## Converting your conda environment to a Singularity container
This theme implements a built-in Jekyll feature, the use of Rouge, for syntax highlighting.
It supports more than 100 languages.
This example is in C++.
All you have to do is wrap your code in markdown code tags:

````markdown
```c++
code code code
```
````

```c++
int main(int argc, char const \*argv[])
{
    string myString;

    cout << "input a string: ";
    getline(cin, myString);
    int length = myString.length();

    char charArray = new char * [length];

    charArray = myString;
    for(int i = 0; i < length; ++i){
        cout << charArray[i] << " ";
    }

    return 0;
}
```

For displaying code in a list item, you have to be aware of the indentation, as stated in [this FAQ](https://github.com/planetjekyll/quickrefs/blob/master/FAQ.md#q-how-can-i-get-backtick-fenced-code-blocks-eg--working-inside-lists-with-kramdown). You must indent your code by **(3 \* bullet_indent_level)** spaces. This is because kramdown (the markdown engine used by Jekyll) indentation for the code block in lists is determined by the column number of the first non-space character after the list item marker. For example:

````markdown
1. We can put fenced code blocks inside nested bullets, too.

   1. Like this:

      ```c
      printf("Hello, World!");
      ```

   2. The key is to indent your fenced block in the same line as the first character of the line.
````

Which displays:

1. We can put fenced code blocks inside nested bullets, too.

   1. Like this:

      ```c
      printf("Hello, World!");
      ```

   2. The key is to indent your fenced block in the same line as the first character of the line.

By default, it does not display line numbers. If you want to display line numbers for every code block, you can set `kramdown.syntax_highlighter_opts.block.line_numbers` to true in your `_config.yml` file.

If you want to display line numbers for a specific code block, all you have to do is wrap your code in a liquid tag:

{% raw %}
{% highlight c++ linenos %} <br/> code code code <br/> {% endhighlight %}
{% endraw %}

The keyword `linenos` triggers display of line numbers.
Produces something like this:

{% highlight c++ linenos %}

int main(int argc, char const \*argv[])
{
string myString;

    cout << "input a string: ";
    getline(cin, myString);
    int length = myString.length();

    char charArray = new char * [length];

    charArray = myString;
    for(int i = 0; i < length; ++i){
        cout << charArray[i] << " ";
    }

    return 0;

}

{% endhighlight %}
