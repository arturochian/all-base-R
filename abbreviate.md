---
layout: page
permalink: /all-base-R/abbreviate/
---

## __abbreviate__

#### _Abbreviate Strings_

Abbreviate strings to at least minlength characters, such that they remain unique (if they were), unless `strict = TRUE`.

### Usage

{% highlight r %}
abbreviate(names.arg, minlength = 4, use.classes = TRUE,
           dot = FALSE, strict = FALSE,
           method = c("left.kept", "both.sides"))
{% endhighlight %}

### Arguments

`names.arg`

a character vector of names to be abbreviated, or an object to be coerced to a character vector by as.character.

`minlength`

the minimum length of the abbreviations.

`use.classes`

logical (currently ignored by R).

`dot`

logical: should a dot (".") be appended?

`strict`

logical: should minlength be observed strictly? Note that setting strict = TRUE may return non-unique strings.

`method`

a string specifying the method used with default "left.kept", see ‘Details’ below.

### Notes

The default algorithm (`method = "left.kept"`) used is similar to that of S. For a single string it works as follows. First all spaces at the beginning of the string are stripped. Then (if necessary) any other spaces are stripped. Next, lower case vowels are removed (starting at the right) followed by lower case consonants. Finally if the abbreviation is still longer than minlength upper case letters are stripped.

Characters are always stripped from the end of the word first. If an element of names.arg contains more than one word (words are separated by space) then at least one letter from each word will be retained.

Missing (NA) values are unaltered.

If `use.classes` is FALSE then the only distinction is to be between letters and space. This has NOT been implemented.

### Values

A character vector containing abbreviations for the strings in its first argument. Duplicates in the original names.arg will be given identical abbreviations. If any non-duplicated elements have the same minlength abbreviations then, if method =   "both.sides" the basic internal abbreviate() algorithm is applied to the characterwise reversed strings; if there are still duplicated abbreviations and if strict = FALSE as by default, minlength is incremented by one and new abbreviations are found for those elements only. This process is repeated until all unique elements of names.arg have unique abbreviations.

The character version of names.arg is attached to the returned value as a names argument: no other attributes are retained.

### Warning

This is really only suitable for English, and does not work correctly with non-ASCII characters in multibyte locales. It will warn if used with non-ASCII characters (and required to reduce the length).

### Examples

{% highlight r %}

x <- c("abcd", "efgh", "abce")
abbreviate(x, 2)
abbreviate(x, 2, strict = TRUE) # >> 1st and 3rd are == "ab"
 
(st.abb <- abbreviate(state.name, 2))
table(nchar(st.abb)) # out of 50, 3 need 4 letters
as <- abbreviate(state.name, 3, strict = TRUE)
as[which(as == "Mss")]
 
# method="both.sides" helps: no 4-letters, and only 4 3-letters
st.ab2 <- abbreviate(state.name, 2, method = "both")
table(nchar(st.ab2))

# Compare the two methods
cbind(st.abb, st.ab2)

{% endhighlight %}
