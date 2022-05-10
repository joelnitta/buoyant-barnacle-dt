# buoyant-barnacle-dt
    
This is the lesson repository for buoyant-barnacle, modified to demonstrate usage
of the [dovetail R package](https://github.com/joelnitta/dovetail) for translating
Carpentries lessons.

`dovetail` is not yet on CRAN, and needs to be installed from GitHub:

```r
devtools::install_github("joelnitta/dovetail")
```

The below code demonstrates how you can generate the files needed for translation:

```r
library(dovetail)

# Copy (untranslated) files needed for rendering lesson to locale/{lang}/
# This will overwrite any translated files currently there.
create_locale("ja")

# Create PO files ----
# Either one file at a time; e.g., episode file
# create_po("episodes/01-introduction.Rmd", "po/ja/01-introduction.po")

# Or (recommended), create all PO files at once in `./po/{lang}/`
create_po_for_locale("ja")

# Edit PO files ----
# for example, with
# usethis::edit_file("po/ja/01-introduction.po")
# usethis::edit_file("po/ja/index.po")
# etc...

# Translate md files ----
# Either one at a time:
# translate_md(
#   md_in = "episodes/01-introduction.Rmd",
#   po = "po/01-introduction.ja.po",
#   md_out = "locale/ja/episodes/01-introduction.Rmd")

# Or (recommended) translate all (R)md files at once to `./locale/{lang}/`
translate_md_for_locale("ja")

# Note that dovetail will still "translate" markdown files without any
# msgtr strings in the PO file. In other words, untranslated strings
# will show up in the output markdown file in their original language.

# Build translated lesson ----
sandpaper::build_lesson("locale/ja/")
```

If you run the above code without adding translations to the PO files, the 
"translated" lesson will appear exactly like the original lesson (in English).

I have provided some PO files with a few strings translated into Japanese
to demonstrate how this works in the [ja branch](https://github.com/joelnitta/buoyant-barnacle-dt/tree/ja).

If you check out the `ja` branch and run `translate_md_for_locale("ja")` followed
by `sandpaper::build_lesson("locale/ja/")`, it will build the (partially) translated
lesson in `locale/ja/site`.
