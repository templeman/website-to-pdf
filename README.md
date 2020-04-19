# Website-To-PDF

> Convert any[*](#known-issues-and-limitations) website to a PDF via the command line

## Overview

Website-To-PDF is a wrapper for a number of popular command line tools that
specialize in capturing screenshots and manipulating images and PDF files. This
wrapper automates the process of generating a high-fidelity PDF from a website,
without the need for a browser or GUI.

Specifically, the script will

1. Accept a seed URL and use it to build out a list of URLs for that site.
2. For each URL, fetch as a screenshot using APIFlash, a screenshot-generator API service.
3. Combine all resultant screenshots one below another vertically into a single image.
4. Chop the aggregate image into equal, letter-sized images.
5. Combine the images into a single PDF file.
6. Run OCR on the PDF to make it searchable.

## Requirements

This script requires the following:

- An [ApiFlash](https://apiflash.com/) account for generating screenshots. ApiFlash offers a free tier with 100 screenshots/month.
- A Bourne-compatible shell (only tested for zsh)
- [Homebrew](https://brew.sh) package manager for macOS
- [Enquirer](https://github.com/termapps/enquirer)
- [LinkChecker](https://github.com/linkchecker/linkchecker)
- [ImageMagick](https://imagemagick.org/index.php)
- [OCRmyPDF](https://github.com/jbarlow83/OCRmyPDF)
- [img2pdf](https://github.com/josch/img2pdf)

Website-To-PDF will check that these requirements are met and, if not, will
attempt to install the missing resources.

## Install

1. Download the file `website_to_pdf`
2. Make it executable with `chmod +x website_to_pdf`
3. Rename the file `.env-sample` to `.env` and record your personal API key from ApiFlash therein.

## Usage

Invoke the script from the command line
```
$ ./website_to_pdf
```
and follow the prompts. Upon successful completion of the script, a new directory containing
the output PDF will be created relative to the `website_to_pdf` file.

### Prompts

**Seed URL:** The full base URL of the website that you want to convert, e.g.
https://somewebsite.com.

**Get fresh screenshots:** If `yes`, force ApiFlash to capture new screenshots,
otherwise use cached images if available. Default is `no` (use cached if
available).

**Dry run:** If `yes`, run the script without actually generating screenshots
or manipulating files/directories. Useful for testing. Default is `no`.

**Preserve page breaks:** If `yes`, the generated PDF will match up with the
source website 1-to-1, meaning that Website-To-PDF will fit each webpage into
an 8.5x11in page in the output PDF. The implication here is that if the source
webpage is larger than 8.5x11in it will be sized down to fit those dimensions
within the PDF, resulting in inconsistent sizes of content. If `no`,
Website-To-PDF will ignore page breaks and chop the website into equal,
letter-sized chunks that fit evenly into the PDF. The downside with this method
is that there is a likelyhood of awkward page breaks within the PDF, but all of
the content will remain a consistent size. Default is `no`.

**Sort order of pages:** This prompt allows for manually reordering the
captured source URLs, which by extension controls the page order of the output
PDF file.

**Sort order usage**
- Arrow keys to move up/down the list
- `SPACE` to 'grab' a list item
- `SPACE` again to 'release' a list item
- `ENTER` to accept

## Known Issues and Limitations

Website-To-PDF will only capture URLs from *one level of a site hierarchy*. In
other words, the script is non-recursive. This is intentional as the script is
not designed to handle the thousands of URLs and screenshots that would be
generated from a large site. However, it is possible to tweak this value - look
at the `linkchecker` command, specifically the `r` flag which sets recursion
depth.

Website-To-PDF was created for an internal project that involved URLs of a
specific, nonvarying structure. As a result, a major idiosyncrasy of the script
is that it will use characters 9-19 of the seed URL to generate the filename of
the resulting PDF. Customizing this filename is left as an exercise for the
user.

## Disclaimer

This script is being made availabe in the hopes that it will prove useful to
others looking for a simple and reliable way to convert a website to a PDF. It
is meant to be modified and adapted to invidual needs and, as such, it is
provided here on an “as-is” basis. No warranties are made regarding its
effectiveness and the author disclaims liability for damages resulting from its
use. Refer to the [License](LICENSE.md) for details.
