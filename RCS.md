RCS
===

Suppose you have a file `f.c` that you wish to put under control of RCS. If you have not already done so, make an RCS directory with the command:

     mkdir RCS

Then invoke the checkin command:

     ci f.c

This command creates an RCS file in directory `RCS`, stores `f.c` into it as revision 1.1, and deletes `f.c`. It also asks you for a description. The description should be a synopsis of the contents of the file. All later checkin commands will ask you for a log entry, which should summarize the changes that you made.

To get back the working file f.c in the previous example, use the checkout command:

     co f.c

This command extracts the latest revision from the RCS file and writes it into `f.c`. If you want to edit `f.c`, you must lock it as you check it out, with the command:

     co -l f.c

You can now edit `f.c`. Suppose after some editing you want to know what changes that you have made. The command:

     rcsdiff f.c

tells you the difference between the most recently checked-in version and the working file. You can check the file back in by invoking:

     ci f.c

This increments the revision number properly. If ci complains with the message:

     ci error: no lock set by your name

then you have tried to check in a file even though you did not lock it when you checked it out. Of course, it is too late now to do the checkout with locking, because another checkout would overwrite your modifications. Instead, invoke:

     rcs -l f.c

This command will lock the latest revision for you, unless somebody else got ahead of you already. In this case, you'll have to negotiate with that person.

Locking assures that you, and only you, can check in the next update, and avoids nasty problems if several people work on the same file. Even if a revision is locked, it can still be checked out for reading, compiling, etc. All that locking prevents is a checkin by anybody but the locker.

If your RCS file is private, i.e., if you are the only person who is going to deposit revisions into it, strict locking is not needed and you can turn it off. If strict locking is turned off, the owner of the RCS file need not have a lock for checkin; all others still do. Turning strict locking off and on is done with the commands:

     rcs -U f.c    # disable strict locking
     rcs -L f.c    # enable strict locking

If you don't want to clutter your working directory with RCS files, create a subdirectory called `RCS` in your working directory, and move all your RCS files there. RCS commands will look first into that directory to find needed files. All the commands discussed above will still work, without any modification.

To avoid the deletion of the working file during checkin (in case you want to continue editing or compiling), invoke one of:

     ci -l f.c     # checkin + locked checkout
     ci -u f.c     # checkin + unlocked checkout

These commands check in `f.c` as usual, then perform an implicit checkout. The first form also locks the checked in revision, the second one doesn't. Thus, these options save you one checkout operation. The first form is useful if you want to continue editing, the second one if you just want to read the file. Both update the keyword substitutions in your working file see Concepts.

You can give `ci` the number you want assigned to a checked-in revision. Assume all your revisions were numbered 1.1, 1.2, 1.3, etc., and you would like to start release 2. Either of the commands:

     ci -r2 f.c
     ci -r2.1 f.c

assigns the number 2.1 to the new revision. From then on, `ci` will number the subsequent revisions with 2.2, 2.3, etc. The corresponding co commands:

     co -r2 f.c
     co -r2.1 f.c

retrieve the latest revision numbered 2.x and the revision 2.1, respectively. `co` without a revision number selects the latest revision on the trunk, i.e. the highest revision with a number consisting of two fields. Numbers with more than two fields are needed for branches. For example, to start a branch at revision 1.3, invoke:

     ci -r1.3.1 f.c

This command starts a branch numbered 1 at revision 1.3, and assigns the number 1.3.1.1 to the new revision. Here is a diagram showing the new revision in relation to its branch and the trunk.

     1.1  --  1.2  --  1.3  --  1.4  --  1.5
                        |
                     [1.3.1]  --  1.3.1.1

