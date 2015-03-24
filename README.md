Stuff I Routinely Forget How to Do
==================================

I need an internet connection to program well because I'm constantly looking up things I've forgotten. Thankfully, all that information is stored in my ["outboard brain"](http://www.wired.com/techbiz/people/magazine/15-10/st_thompson) searchable by Google. I figured I'd collect the useful bits into a growing list, in the hope that others may benefit from my documented forgetfulness.

It's really a letter of advice from Past Christina to Future Christina.

Quick Links
-----------

- [Perl](#Perl)
- [R](#R)
- [Unix/bash](#Unix)
- [CSS](#CSS)
- [HPC](#HPC)
- [git](#git)

<a name="Perl"></a>
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

-   **Quickly Create Hash of Arrays**
    
    Load the arrays in the hash without building the arrays, then dereferencing them into the hash. Found [here](http://www.perlmonks.org/?node_id=383463).
    
    ```perl
    push(@{$hash{$key}}, $insert_val);
    ```

<a name="R"></a>
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

-   **Using Variables within `expression()`**
    
    See this [answer](http://stackoverflow.com/questions/15074127/use-expression-with-a-variable-r/15074283#15074283) on StackOverflow.
    
    ```r
    alpha = 0.05
    bquote("Representation in Low"~F[ST]~Regions~"(p"*""<=~.(alpha)*")")
    ```
    
    "The `~` is a spacing operator and `*` means juxtapose the contents to the left and right of the operator. In `bquote()`, anything wrapped in `.( )` will be looked up and replaced with the value of the named object; so `.(assay)` will be replaced."

-   **Create Function with Range of Possible Parameters and Default**
    
    Description can be found [here](http://stackoverflow.com/a/4684604). In this function, age can only be adult or subadult and defaults to adult:
    
    ```r
    my.function = function(name, age = c("adult", "subadult")) {
        ## evaluate choices
        age = match.arg(age)
        print(age)  # Defaults to adult
    }
    ```

<a name="Unix"></a>
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

-   **List files and sort numbers "naturally"**
    
    [Natural sort](http://www.naturalordersort.org/) means sorted like this:
    
        chr1.fa
        chr2.fa
        chr10.fa
    
    instead of this:
    
        chr1.fa
        chr10.fa
        chr2.fa
    
    You can do this with `ls -v`. Using this natural sort with `xargs` is super handy for `cat`'ing together chromosomes in order:
    
    ```bash
    ls -v chr*.fa | xargs cat > genome.fa
    ```

-   **Keyboard Shortcuts**
    
    A subset of the ones found [here](http://www.computerhope.com/ushort.htm) and [here](http://unix.stackexchange.com/a/49).
    
    Combo     | Action
    -------   | --------------------------------------------------------------------
    `CTRL+S`  | Stops output on screen (XOFF).
    `CTRL+Q`  | Turns stopped output back on (XON).
    `CTRL+U`  | Erases (cuts) the complete line.
    `CTRL+Y`  | Pastes line cut with CTRL+U.
    `CTRL+W`  | Deletes the last word typed. `mv file1 file2` becomes `mv file1`.
    `CTRL+L`  | Clears the screen, redrawing the current line.
    `CTRL+G`  | Abandons the current line
    
-   **Recall Last Argument of Previous Command**
    
    Quickly grab the last command's argument(s). See this [thread](http://stackoverflow.com/questions/3371294/how-can-i-recall-the-argument-of-the-previous-bash-command):
    
    Variable     | Meaning
    -------------|--------------------------------
    `$_` or `!$` | Last argument (also `Alt + .`)
    `!:1`, `!:2` | First, second argument, etc.
    `!:3-4`      | Third and fourth arguments 
	`!:0`        | Previous command
	`!*`         | All arguments to last command

-   **Find Recently Modified Files**
    
    Find files modified less than 15 minutes ago:
    
    ```bash
    find / -mmin -15
    ```

-   **Find and Delete Files Older than x Days**
    
    You can see files older than 20 days with:
    
    ```bash
    find results/bgc/2000loci/* -mtime +20
    ```
    
    If you want to do something to those files, add `-exec`, the command, and then `{} \;`. For instance, this applies `ls -lh` to files older than 20 days:
    
    ```bash
    find results/bgc/2000loci/* -mtime +20 -exec ls -lh {} \;
    ```
    
    And this deletes them:
    
    ```bash
    find results/bgc/2000loci/* -mtime +20 -exec rm {} \;
    ```

-   **Duplicate `STDOUT` to `STDERR`**
    
    ```bash
    echo “Whatev” | tee /dev/stderr
    ```

-   **Add Newline to End of File**
    
    Adds `\n` at the end of the file if needed. Works on OSX.
    
    ```bash
    sed -i '' -e '$a\' faulty_file.txt
    ```

-   **Curly Bracket Expansion**
    
    Anything in braces expands:
    
    ```bash
    cp important_file.txt{,_backup}
    rm really/long/path/{alpha,beta}
    touch {a..k}.txt
    rm section-{1..9}.txt
    ```

-   **Circumflex Command Editing**
    
    Not sure what the name of this is, but useful for typos and changing parameters in long commands. Note that only first instance is changed.
    
    ```bash
    head ~/Desktop/tmp_line_counts.txt
    # ...
    ^Desktop^Documents
    # Runs "head ~/Documents/tmp_line_counts.txt "
    ```

<a name="CSS"></a>
CSS
---

-   **Setting margins**

    Order is Top, Right, Bottom, Left. TRBL! Mnemonic: TReBLe clef.

    ```css
    margin: 0px 25px 50px 75px;
    ```

<a name="HPC"></a>
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

    which would run with 0, 3, 6, and 9 as `PBS_ARRAYID`. A handy trick
    for using array jobs to pull input file names out of a text file is:

    ```bash
    INPUT_FILE=`awk "NR==$PBS_ARRAYID" file_list.txt`
    ```

    All of this info was stolen from the [NYU HPC
    Wiki](https://wikis.nyu.edu/display/NYUHPC/Tutorial+-+Submitting+a+job+using+qsub#Tutorial-Submittingajobusingqsub-arrayjob).

-   **PBS Environmental Variables**
    
    Good reference can be found [here](http://www.arl.hpc.mil/docs/pbsUserGuide.html). For instance, `$PBS_NODEFILE` is the path to a file containing a list of available nodes:
    
    ```bash
    echo `wc -l ${PBS_NODEFILE} | cut -f1 -d" "`
    ```

<a name="git"></a>
git
---

-   **Save Progress without Commit**
    
    `git stash` ([see here](http://git-scm.com/book/en/v1/Git-Tools-Stashing)) creates a stack of unfinished changes so you can reapply them later when you're ready to keep working on the half-completed step.    

-   **Undo the Last Commit**
    
    Undo the last commit and move all changes back to stage. (Must be done before push.)
    
    ```bash
    git reset --soft HEAD^
    ```

-   **Undo the Last Commit and REMOVE Changes**
    
    Undo the last commit and REMOVE all changes. (Must be done before push.)
    
    ```bash
    git reset --hard HEAD^
    ```

-   **Undo Multiple Commits and REMOVE Changes**
    
    Undo the last two commits and REMOVE all changes. (Must be done before push.)
    
    ```bash
    git reset --hard HEAD^^
    ```

-   **Amend Last Commit**
    
    Amend the last commit. (Must be done before push.) Optional commit message will overwrite the original commit message.
    
    ```bash
    git add whatev.txt
    git commit --amend -m "Add foo.txt and modify whatev.txt"
    ```

-  **Reset Unstaged Changes**
    
    ```bash
    git checkout file_to_revert.txt
    ```

-  **Intelligently Show LaTeX Changes**
    
    ```bash
    git --no-pager diff --color-words manuscript.tex
    ```
