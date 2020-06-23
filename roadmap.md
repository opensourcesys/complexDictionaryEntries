# Roadmap / Design Ideas

*Last updated: 6/23/2020*

Below are two possible UX designs, and then a description of what would have to happen behind the scenes.

## UX Method 1:

<!-- This is the start of my original method. I have ditched it in favor of what is below. This remains in case I choose to revive this bad idea some day.

* Add a combobox to the dictionary "add" dialogs, with two options:
    + Standard: The dictionaries work as normal.
    + Complex entry: causes a formerly invisible combobox to appear containing the options: pattern 1, pattern 2, ..., pattern 6, replacement.
        - For each of the "pattern number" choices, the user can set a pattern, but can't set a replacement.
        - For the "replacement" selection, the user can set a replacement, but no pattern.
        - The Comment field becomes mandatory for complex entries.
* Whenever the dictionary is edited, code will run that will:
    + Aggregate all complex entries that have the identical comment.
-->

* Replace the dictionary dialog "add" button, with two buttons.
    + Add standard entry: identical to the current add button.
    + Add complex entry: presents a new add dialog, as follows:
        - Has six (an arbitrary number) pattern fields, all optional but the first two.
        - Could possibly include a button to add more pattern fields.
        - An alternative would be a textarea allowing newline separated entries, as many as desired.
        - The rest of the add dialog would be unchanged: replacement field, case sensitivity, etc.
* The above complex add dialog, would also show up for edit operations on complex entries.

## UX method 2:

Keep the current dictionary add dialog.
Implement a completely internal processor, that recognizes a code which users can place between entries.
That would then be broken into separate patterns behind the scenes.

As an example, the user could enter the following pattern:

```
Annoying text###link###heading###level \d
```

In this case, a regular expression entry, the "###" (three number signs) would represent the pattern breaks.

Alternatively, it could be something like:

```
Annoying text<>link<>heading<>level \d
```

With a diamond operator doing the separation.

This is clean from a UI point of view, but it means users really have to know what their doing to use this feature.

It might also be easier to implement as an initial proof of concept.

## Behind the scenes

1.  When a speech string matches pattern 1:
    + It gets added to a pattern buffer.
    + Nothing is sent to the synth.
2.  The next utterance is checked for matches against the next pattern of the previously matched entry.
    + If it fails, this is not a match, hand pattern 1, and then patterns 2 through N , off to regular dictionary and speech processing, as if they just came in.
    + If it matches, and there are no more patterns for this entry:
        - Execute the replacement on the string constructed from the patterns, separated by two spaces, as it was in the old days.
        - Pass the result to the regular speech chain, ultimately going to the synth.
    + If it matches, but there is at least one more pattern, push this pattern to the pattern buffer, and rerun step 2, with the next pattern on the next utterance.
    
    There is at least one obvious problem with all of this.

While we're waiting for patterns 2 through N to be checked, pattern 1, and then patterns 2 through N-1, are not being spoken.

The entire concept of this add-on assumes that it is working with a near instantaneous stream of speech.
I believe we need to implement a very short duration timer to handle the case where pattern 1 matches, but then there is no more speech, and we don't get a pattern 2 (or whichever further pattern), and then have that timer flush the pattern buffer to the second half of step 2 above, and thereby end processing of the entry.
That, I think, will necessitate a thread. If there's any other way to handle this, I'd love to find it..
If NVDA has a way to indicate internally that no more speech is coming, I don't know about it yet; but I have only just started researching the speech refactor code.

## Final thoughts

It is possible that the whole point of speech refactor--that of making it easier to work with bits of speech and take non-speech actions on them--could make this a lot easier than I described above.
If, for example, there is a way to "tag" bits of speech already extant, we might not have to build the pattern buffer here, and so on.
I simply don't know yet.
This document, especially the "Behind the scenes" section, is entirely a work in progress.
