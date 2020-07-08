# dream-eater

`dream-eater`. A minor mode for working with Dreamweaver users. This
mode is intended to work alongside Tramp.

This minor mode changes the behaviour of editing files. When
`dream-eater-mode` is on, all files will be opened in read only mode.

Files may only be edited when they are checked out. Checking out a
file lets other users know that someone else is currently editing that
file. This behaviour avoids conficts with multiple people working on
the same codebase.

To free up write access to a file and let other users edit the file,
it must be checked in. Checking in also has the side effect of saving
your changes if they are not saved already.

## Usage:

Setup your name and email to allow Dreamweaver and other users to
identify you:

```emacs-lisp
(defvar dream-eater/check-out-name "Your name")
(defvar dream-eater/email "your@email.com")
```

To edit a buffer, it must be checked out by running
`dream-eater/check-out` while viewing the buffer.

> Unlike the same action in Dreamweaver, `dream-eater/check-out` will
  never let you override someone else's check out.

When you are done making changes, run `dream-eater/check-in` to allow
others to check out the same file.

If you want to save changes but continue editing, simpily save the
buffer. This is the same action as "Put" on Dreamweaver. When
`dream-eater-mode` is on, `save-buffer` is replaced with
`dream-eater/put`.

## Explanation

Dreamweaver creates a lock file (`<name-of-file>.LCK`) whenever
someone checks out a file. The lock file is in plain text with the
following format:

```
Your Name||your@email.com
```

`dream-eater/check-out` creates a lock file for the current buffer. It
will not create a lock file if someone else has already checked out
the associated file.

`dream-eater/check-in` will remove the lock file if you own it. Check
in will display a message if someone else has checked out a file, or
if the lock file does not exist.

`dream-eater/put` is a wrapper for `save-buffer`. It will refuse to
save your changes if you don't own the lock file.
