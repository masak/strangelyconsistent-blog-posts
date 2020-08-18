# The curse of "Save"

A child of the diskette era, I remember saving with some fondness.
And the action made sense, what with the cold economics of primary and secondary memory back then.
"Saving" implies preservation, rescuing from certain oblivion.
It makes sense to want to put ones data somewhere safe.
It made sense to hit "Save", and often, as often as the balance allowed.

It doesn't make much sense anymore.

So, I have this application on my (Huawei) Android phone, Notepad.
This is the second time Nodepad figures in these diaries &mdash; didn't expect that, to be honest, but maybe it's fair.
I use it quite a bit, and for exactly its stated skeumorphic purpose: as a pad for notes.

But a real pad of notes has never asked me to save its contents.
Notepad does.

It has a perky little checkbox up in its top-right corner.
Not a save icon, to its credit, just a checkbox.
To needlessly confirm that the changes I just made to my notes were the kind of changes I'm interested in preserving.

If I try to exit a particular note ("document"?) without tapping the checkbox, Notepad drops all caution to the wind and straight-out asks me:
"Save your changes? [DISCARD/SAVE]"
It's kind of the UI equivalent of a person who snatches your car keys, holds them provocatively at arm's length, and says "I want to hear the magic word!"

Interestingly, if I force-quit Notepad (by closing it in the application chooser, or by turning off my phone), it dutifully saves the note I had open.
You know, just in case there was something important I wanted saved.

Let's recap: the "Save" action used to make sense when secondary storage was slow and unreliable.
Floppy disks (and the not-so-floppy disks that succeeded them) were very slow and very unreliable.
Spinning-rust hard drives were only moderately slow and almost not unreliable.
SSD storage is pretty fast and reliable as long as you don't do something stupid with it.
Then we all woke up one morning, and noticed that the "Save" action didn't make sense.
Or failed to notice, as the case may be.

To be precise, it's not saving itself that doesn't make sense.
Saving makes a lot of sense; that's the thing.
Apps shouldn't be asking me to save &mdash; they should just do it.

Everything should be saved, all the time, always.
Convince me otherwise.

iOS applications seem to be very good at realizing this, for some reason.
I dunno, maybe the "Think Different" culture they have brewing over at Apple &mdash; and well-thought-out user interface guidelines &mdash; make it easier to see that "Save" isn't a meaningful action any more.

Various cloud applications generally do well, too.
Things like Google Docs, or OneNote.
I'm find with a small "Saving..." message automatically showing up sometimes.
If I could wish for the stars, I would like to have _both_ continuous saving for when I'm online _and_ offline editing for when I'm not.
Seems we're not quite there yet; most cloud applications are cloud-only, not offline-first.

I even believe &mdash; though I haven't proven this by trying it out in practice &mdash; that saving, and the notion of files, can go away completely once Git is in the picture.

Git traditionally has three "levels" where changes exist:

1. The working copy, which is files "checked out" into a real directory on your hard drive
2. The staging area; changes which you "are about to commit" hang out here
3. Commits in history; changes that you have already committed

But one of these things are not like the other: level 1, the working copy, belongs to the old world of drives and files and saving; levels 2 and 3 belong to various levels of version control.

(The staging area gets a lot of flak by people who neither like nor understand it, but I like it, and I use it quite a lot.
Sure, it's not absolutely necessary, but I feel it's a nice help.)

Do we need the working copy?
Certainly not; its only role is to hold unstaged changes for a _really_ short time before you stage them.
The editor can do that; we get rid of files; Git gets simpler; everybody wins.

Maybe you might miss being able to persist ongoing changes between editor sessions.
But Git already has various mechanisms for that: stashing, temporary branches, more permanent branches.
The editor (or IDE) wouldn't even really need to ask for your confirmation; it would just commit your work somewhere in a convenient form, and then restore it next time.

