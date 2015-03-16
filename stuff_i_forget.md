Stuff I Routinely Forget How to Do
==================================

I need an internet connection to program well because I'm constantly looking up things I've forgotten. Thankfully, all that information is stored in my ["outboard brain"](http://www.wired.com/techbiz/people/magazine/15-10/st_thompson) searchable by Google. I figured I'd collect the useful bits into a growing list, in the hope that others may benefit from my documented forgetfulness.

It's really a letter of advice from Past Christina to Future Christina.

Perl
----

-   **For each file in a directory**

    Capture a list of all files in a directory with the glob function:

    ```perl
    @files = <*>;
    @txt_files = <my_dir/*.txt>;

    foreach $f (@files) {
        print $f . "\n";
    } 
    ```

-   **Push items into array**

    Why can I never remember the order of this? It's:

    ```perl
    push @array, $element;
    ```

-   **Split string into array of strings**

    Another one I alway mess up:

    ```perl
    split /PATTERN/,EXPR,LIMIT
    ```

-   **Install a module with CPAN**

    ```bash
    perl -MCPAN -e 'install X::Y'
    ```

-   **Install a module with CPAN in a non-standard location**

    ```
    o conf mbuildpl_arg "--install_base /path/to/perl-modules/"
    o conf makepl_arg "PREFIX=/path/to/perl-modules/"
    install X::Y
    ```

R
-

-   **Remove NAs from a vector**

    ```r
    x[!is.na(x)]
    ```

-   **Combine multiple plots in one graph**

    ```r
    par(mfrow=c(num.rows, num.cols))
    ```

-   **Generate the R code needed to reproduce an object**

    ```r
    dput(head(iris))
    ```

Unix/bash
---------

-   **Sort by date**

    Use the -t flag with ls to sort by date files were last modified (or
    -tr to sort in reverse).

    ```bash
    ls -lht
    ```

-   **Use process substitution**

    Explained a bit more
    [here](http://christinabergey.com/blog/2013/08/process-substitution/).

    ```bash
    diff <(ls /scratch/secret_project/) <(ls /archive/secret_project/)
    ```

CSS
---

-   **Setting margins**

    Order is Top, Right, Bottom, Left. TRBL! Mnemonic: TReBLe clef.

    ```css
    margin: 0px 25px 50px 75px;
    ```

HPC
---

-   **Loading Modules, Seeing Effects**

    `module avail` gives you the available modules.

    `module load [module name]` loads the module.

    `module display [module name]` tells you what the module will do to
    you environment.

    ```bash
    $ module display abyss/intel/1.3.2
    ---------------------------------------------------------------
    /share/apps/modules/modulefiles/abyss/intel/1.3.2:

    module-whatis ABySS: Assembly By Short Sequences - a de novo... 
    conflict      abyss 
    module        load intel/11.1.046 
    module        load openmpi/intel/1.4.3 
    prepend-path  PATH /share/apps/abyss/1.3.2/intel/bin 
    prepend-path  MANPATH /share/apps/abyss/1.3.2/intel/share/man 
    setenv        ABYSS_ROOT /share/apps/abyss/1.3.2/intel 
    ---------------------------------------------------------------
    ```

-   **Submitting Array Jobs**

    You can submit array jobs with `qsub` using the `-t` flag and a
    range of numbers or commas:

    ```bash
    $ qsub -t 0-100 myjob.pbs
    ```

    The job array index is stored in `PBS_ARRAYID`. NYU has a handy
    wrapper that lets you submit jobs in a range of numbers with a step
    size:

    ```bash
    $ qsub -t 0-10:3 myjob.pbs
    ```

    which would run with 0, 3, 6, and 9 as PBS\_ARRAYID. A handy trick
    for using array jobs to pull input file names out of a text file is:

    ```bash
    INPUT_FILE=`awk "NR==$PBS_ARRAYID" file_list.txt`
    ```

    All of this info was stolen from the [NYU HPC
    Wiki](https://wikis.nyu.edu/display/NYUHPC/Tutorial+-+Submitting+a+job+using+qsub#Tutorial-Submittingajobusingqsub-arrayjob).


