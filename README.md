# Send git patches with custom message ids

As usual this is the result of nerd sniping. A particularly bad case.

[Konstanin Ryabitsev](https://social.kernel.org/monsieuricon) has done a lot of great work to improve tooling for Linux kernel developers.
I encourage everyone to check out [b4](https://b4.docs.kernel.org/en/latest) which is on its way to becoming a standard tool for Linux kernel development.

A fallout from his work was a script to generate custom `Message-Id`s and how to use them in mails sent with [mutt](https://gitlab.com/muttmua/mutt) or [neomutt](https://github.com/neomutt/neomutt)
His [original message](https://mastodon.social/@monsieuricon@social.kernel.org/110102644429872649) contains a link to the script.

Porting my own workflow to use custom `Message-Id`s I really wanted to use them for patches sent with `git send-email` as well.
While `b4` has mostly replaced `git send-email` it still is useful for sending patches that don't require complex patch series management and so should use uniform `Message-Id`s as well.

And indeed, with some creative tinkering it's possible to use custom `Message-Id`s in `git send-email` as well as I showed in [this thread](https://mastodon.social/@monsieuricon@social.kernel.org/110102644429872649).
After some back and forth between @gregkh, Konstantin, and myself we came to a working version.

This repository contains the necessary bits and pieces.

Place the `generate-msgid` script into a location of your choice.
I tend to place custom binaries in `~/src/bin/` so that's what's in the script.
Make sure to update it accordingly.
You must also change the `Message-Id` format in the `generate-msgid` script.

Place the `sendemail.validate` hook in your git repositories `.git/hooks/` folder.
Note, that the `sendemail.validate` hook doesn't just generate and insert a new `Message-Id` into the patch but also calls `patatt sign --hook` on the final patch.
This is mainly useful for the Linux kernel development workflows which relies on patch attestation.
Should you intend to use this for other projects you want to remove the last line.

The `sendemail.validate` script features here uses Python.
@gregkh has a version that's written in Perl.

Christian
