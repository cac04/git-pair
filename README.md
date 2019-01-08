# Git for Pair Programming

Git commit messages can only have one author. However, there is a convention
that multiple authors can be recorded by lines beginning with `Co-authored-by:`
at the end of a commit message. This convention is [now supported by
GitHub](https://help.github.com/articles/creating-a-commit-with-multiple-authors/)
so all the co-authors can share the internet points.

This POSIX shell git plugin makes it easy to manage pair programming sessions
and automatically add the appropriate `Co-authored-by` trailers to your commit
messages.

## Installation

- Put the `git-pair` script somewhere in your `$PATH`.
- Put the `prepare-commit-msg` script in `.git/hooks/` inside each of your
  repos.

That last step is a bit tedious. I suggest you create a shell alias like so:
```
alias git-pair-init="cp /somewhere/prepare-commit-msg .git/hooks"
```

## Usage

```sh
$ git pair add rh "Robin Hood <robin@sherwood.org.uk>"
$ git pair add mm "Maid Marion <marion@sherwood.org.uk>"
$ git pair add ft "Friar Tuck <tuck@sherwood.org.uk>"
$ git pair add lj "Little John <john@sherwood.org.uk>"
$ git pair list
rh Robin Hood <robin@sherwood.org.uk>
mm Maid Marion <marion@sherwood.org.uk>
ft Friar Tuck <tuck@sherwood.org.uk>
lj Little John <john@sherwood.org.uk>

$ git pair show
No pairs currently set.
$ git pair set mm ft
$ git pair show
Maid Marion <marion@sherwood.org.uk>
Friar Tuck <tuck@sherwood.org.uk>

$ git commit -m 'Something informative'
Committing with co-authors:
 + Maid Marion <marion@sherwood.org.uk>
 + Friar Tuck <tuck@sherwood.org.uk>
[master d898f8d] Something informative
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 foo
$ git log -n1
commit d898f8d3d83759843d19bf71b78ce6089929ccce (HEAD -> master)
Author: Your Usual Git Username <user.email@gitconfig>
Date:   Sat Oct 27 14:17:30 2018 +0100

    Something informative

    Co-authored-by: Maid Marion <marion@sherwood.org.uk>
    Co-authored-by: Friar Tuck <tuck@sherwood.org.uk>
```

## Notes

The list of possible authors is stored in your global `.gitconfig`. The list of
currently active authors is stored in the local `.git/config` of your current
repo. This way you can have different authors active in different repos at the
same time, but you can share their contact details across all repos.
