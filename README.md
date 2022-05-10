# buoyant-barnacle-dt
    
This is the repository for the [Carpentries demo lesson](https://carpentries.github.io/sandpaper-docs/introduction.html) `buoyant-barnacle`, 
modified to demonstrate usage
of the [dovetail R package](https://github.com/joelnitta/dovetail) for translating
Carpentries lessons.

## About [dovetail](https://github.com/joelnitta/dovetail)

`dovetail` is not yet on CRAN, and needs to be installed from GitHub:

```r
devtools::install_github("joelnitta/dovetail")
```

Also, note that `dovetail` relies on external software ([mdpo](https://github.com/mondeja/mdpo)) bundled in a [Docker image](https://hub.docker.com/r/joelnitta/mdpo), 
and therefore requires Docker to be installed and running.

## Example translation

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

## File hierarchy

`dovetail` starts with the standard [sandpaper](https://carpentries.github.io/sandpaper/index.html)
file hierarchy, and modifies it as follows (indicated at bottom with `NEW`):

```
|-- .gitignore                  # - Ignore everything in the site/ folder
|-- .github/                    # - Scripts used for continuous integration
|   `-- workflows/              #
|       |-- deploy-site.yaml    # -   Build the source files on github pages
|       |-- build-md.yaml       # -   Build the markdown files on github pages
|       `-- cron.yaml           # -   reset package cache and test
|-- episodes/                   # - Put your markdown files in this folder
|   |-- data/                   # -   Data for your lesson goes here
|   |-- figures/                # -   All static figures and diagrams are here
|   |-- files/                  # -   Additional files (e.g. handouts) 
|   `-- 00-introducition.Rmd    # -   Lessons start with a two-digit number
|-- instructors/                # - Information for Instructors
|-- learners/                   # - Information for Learners
|   `-- setup.md                # -   setup instructions (REQUIRED)
|-- profiles/                   # - Learner and/or Instructor Profiles
|-- site/                       # - This folder is where the rendered markdown files and static site will live
|   `-- README.md               # -   placeholder
|-- config.yaml                 # - Use this to configure commonly used variables
|-- CONTRIBUTING.md             # - Carpentries Rules for Contributions (REQUIRED)
|-- CODE_OF_CONDUCT.md          # - Carpentries Code of Conduct (REQUIRED)
|-- LICENSE.md                  # - Carpentries Licenses (REQUIRED)
|-- README.md                   # - Introduces folks how to use this lesson and where they can find more information.
|-- po                          # - NEW, contains PO files for translation
|   `-- ja/                     # - NEW, each subfolder is named by the target language (e.g., 'ja' for Japanese)
|       |-- 00-introducition.po # - NEW, each md file to be translated gets a corresponding PO file
|       |-- CONTRIBUTING.po     # - NEW, more PO files...
|       |...                    # - NEW, more PO files...
|-- locale                      # - NEW, contains translated files
|   `-- ja/                     # - NEW, each subfolder is named by the target language (e.g., 'ja' for Japanese)
|       |-- CONTRIBUTING.md     # - NEW, dovetail will generate the translated markdown files here
|       |-- site/               # - NEW, the translated, rendered site gets generated here
|           |-- built/          
|           |...               
```






