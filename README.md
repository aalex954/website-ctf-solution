# Personal Site CTF Write-up: The Morse SVG Decoy and the Library of Babel Trail

## Summary

This write-up covers two puzzles embedded in my personal website. The first one looks like the “main” challenge: an oscilloscope-style SVG that can be interpreted as Morse code. A small hint in the image’s alt text calls out a missing “=” character, and decoding the message ultimately lands you on a familiar YouTube URL — it’s a deliberate rickroll and a dead end.

The real puzzle starts on the About page. Viewing source reveals a Base64-encoded hint (“Contemplate the variation of the 23 letters”), which is intended to steer you toward the Library of Babel. From there, the remaining breadcrumbs are hidden in the SVG itself: metadata includes a custom babel:book tag with CDATA that’s Base64-encoded (and then encoded again). Solving it is mostly careful inspection plus straightforward decoding, with the trick being knowing where to look and not getting stuck on the decoy.

---

## Overview

### “Moorse Code SVG” (decoy)

The entry point is an oscilloscope-style SVG. If you inspect the `<img>` tag, the alt text gives away the first trick: the `=` character was “forgotten,” so you’ll need to restore it during decoding.

From there, you can pull the Morse in two ways: either zoom in and visually separate dots/dashes from the waveform, or jump straight to the referenced Moorse2SVG project to grab the underlying Morse string.

When you decode the Morse, it resolves to a partial “flag” that’s really just a YouTube path. Adding the missing piece (the `=` mentioned in the alt text) turns it into the full URL — and it’s the classic rickroll. That’s the punchline: this whole branch is meant to waste a little time and build confidence before the real puzzle starts.



### Library of Babel (real path)

The real trail begins on the About page. Viewing source reveals a meta tag with a Base64 value; decoding it gives the instruction: “Contemplate the variation of the 23 letters.”

That line is the nudge toward the Library of Babel concept, and the write-up points you to the site’s browse interface as the next place to work from.

The more concrete navigation clue is embedded in the SVG itself. Inside the SVG metadata there’s a custom `babel:book` tag containing a Base64 payload that’s been encoded twice. Decoding both layers yields the location cue: **“Volume 32 on Shelf 2 of Wall 3 of Hexagon.”**

From there, the remaining missing piece is the page number, which is hidden using Unicode “tag” characters (invisible characters from the U+E0000–U+E007F range). The write-up notes you can find this hidden value either in a `<meta name="page" ...>` element or embedded inside otherwise-empty code blocks, and it links out to a reference guide (“Hidden-in-Plain-Hex”) for extracting/reading that kind of hidden text.

Once you combine the Library coordinates with the extracted page number and follow the breadcrumbs through the Library interface, the trail ends with a terminal-styled message and an Einstein quote as the final “reward” text.

---

## Walkthrough

### Part 1: Moorse Code SVG (Decoy)

#### The Initial Clue – Alt Text in the SVG

The first breadcrumb is in the HTML itself. Inspect the `<img>` element that embeds the SVG and you’ll spot an unusually specific hint in the alt text:

```html
<img src="/static/img/svg/morse_oscilloscope_style.svg" alt="I forgot to encode the = symbol so youll have to add it back ^_~">
```

That line is doing two things: confirming there’s something to decode, and warning you up front that the decoded output will be missing a literal `=` character.

#### Revealing the Hidden Signal

There are two practical ways to pull the Morse out of the graphic:

1. **Zoom-In Method**
   Zoom in on the “oscilloscope” wave until you can clearly separate short and long marks. Once the spacing becomes obvious, the pattern reads as dots and dashes rather than just a noisy waveform.

2. **Direct Project Reference**
   If you don’t feel like eyeballing it, the site’s related post for the [Moorse2SVG Project](/posts/steganography-moorse-converter/) contains the original Morse payload that was used to generate the SVG.

#### Decoding the Message

With the Morse sequence in hand, decode it:

```plaintext
"..-. .-.. .- --. ---... -.-- --- ..- - ..- -... . .-.-.- -.-. --- -- -..-. .-- .- - -.-. .... ..--.. ...- -.. --.- .-- ....- .-- ----. .-- --. -..- -.-. --.-"
```

The output isn’t a real “flag” so much as a flag-styled URL fragment:

```plaintext
FLAG:YOUTUBE.COM/WATCH?VDQW4W9WGXCQ
```

Now apply the alt-text hint. The only thing you need to “add back” is the missing `=` in the YouTube query parameter, turning `WATCH?V...` into `WATCH?V=...`:

```plaintext
https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

Following it lands on the classic rickroll. That’s the point: Part 1 is a decoy—fun, believable, and intentionally complete—so you don’t waste time trying to force more meaning out of it before moving on to the real trail in Part 2.

---


### Part 2: Library of Babel (Real)

#### Exploring the About Page

The real trail starts on the About page, but it doesn’t announce itself. You have to view source and look for anything that feels out of place. The giveaway is a meta tag containing a Base64 blob:

```xml
<meta name="hint" content="Q29udGVtcGxhdGUgdGhlIHZhcmlhdGlvbiBvZiB0aGUgMjMgbGV0dGVycw==">
```

Decode it and you get:

```plaintext
Contemplate the variation of the 23 letters
```

That’s the nudge toward the Library of Babel idea: a space built around permutations of a limited alphabet, where “every possible page” exists somewhere.

#### Getting to the Library

At this point you’re meant to connect the hint to the online Library of Babel project. The easiest route is just searching the decoded phrase and landing on the site, then using the browse UI since it provides a predictable entry point:

```plaintext
https://libraryofbabel.info/browse.cgi
```

(You can reach the same place other ways, but using the browse page keeps the next steps consistent.)

#### Pulling the Location from the SVG

The SVG doesn’t only contain the decoy—there’s real data buried in it too. Open the SVG as text and inspect the `<metadata>` section. Inside is a custom tag, `babel:book`, wrapping a CDATA chunk that’s Base64-encoded twice:

```xml
<babel:book xmlns:babel="http://libraryofbabel.info/ns#">
  <![CDATA[
    (base64…)
  ]]>
</babel:book>
```

If you decode the CDATA payload two times, it resolves into a plain-text directive. The first line is the important part:

```plaintext
Volume 32 on Shelf 2 of Wall 3 of Hexagon
```

The long string that follows it is the library “address” payload you’ll need for navigation/search (it’s intentionally huge so it doesn’t stand out as a normal clue).

#### Finding the Missing Page Number (Invisible Unicode)

At this point you have most of the location, but you’re still missing one key piece: the page number. That value is hidden in text using Unicode “tag” characters—code points that won’t render visibly in the browser, but still exist in the DOM.

In this challenge, the hidden page value can be recovered from either of these places:

* A meta tag:

```html
<meta name="page" content="">
```

* Or what looks like an empty code block:

```html
<p><code></code></p>
```

Both look blank in the rendered page, but if you copy the surrounding HTML/text and inspect it (or run it through a tool that exposes non-printing characters), the tag-encoded payload shows up. The decoding approach is documented in your reference repo:

[Hidden-in-Plain-Hex](https://github.com/aalex954/Hidden-in-Plain-Hex)

Once you extract that page number, you can combine it with the “Volume / Shelf / Wall / Hexagon” directive and navigate to the exact spot in the Library of Babel interface.

#### Final Reward

Following the full breadcrumb chain—About page hint → Library of Babel → SVG metadata → hidden Unicode page number—eventually drops you onto the final message. It’s presented like terminal output and ends with an Einstein quote:

```bash
Thanks for digging around. For searching. For finding...........................
................................................................................
................................................................................
The most beautiful thing we can experience is the mysterious. It is the source..
of all true art and all science.................................................
................................................................................
Albert Einstein.................................................................
```

