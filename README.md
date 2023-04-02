# Send git patches with custom message ids

As usual this is the result of nerd sniping. A particularly bad one.

@monsieuricon@social.kernel.org has put in a lot of work to improve tooling for Linux kernel developers.
He showed [point out](https://mastodon.social/@brauner/110117442185514710) how he generated custom message ids and used them with `mutt` or `neomutt`.

As part of that discussion I wanted to see whether it would be possible to use custom `Message-Id:`s for `git send-email` as that is often still used to send kernel patches (You should checkout `b4` though.).
And yes, that is possible [as I showed here](https://mastodon.social/@monsieuricon@social.kernel.org/110102644429872649).
However, email being email it took a few tries to get to a working version.

This repository contains the necessary bits and pieces.

Place the `generate-msgid` script into a location of your choice. I tend to place custom binaries in `~/src/bin/`.
Change the format in `generate-msgid` to your preference but make sure to edit it.

Then place the `sendemail.validate` hook in your git repositories `hook` folder at `.git/hooks`.
Note, that the `sendemail.validate` hook doesn't just generate and insert a new `Message-Id:` it also calls `patatt sign --hook` on the final patch including the custom `Message-Id:`.
This should be fine since all of this is targeted mostly at Linux kernel developers.
But if you don't and don't do patch attestation you might want to remove this line.

The `sendemail.validate` script here uses Python @gregkh also has a version that's written in Perl.
Choose your poison, I guess. :)

Christian
