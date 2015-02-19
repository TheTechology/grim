```
                    ,____
                    |---.\
            ___     |    `
           / .-\  ./=)
          |  |"|_/\/|
          ;  |-;| /_|
         / \_| |/ \ |
        /      \/\( |
        |   /  |` ) |
        /   \ _/    |
       /--._/  \    |
       `/|)    |    /
         /     |   |
       .'      |   |
      /         \  |
     (_.-.__.__./  /
```

# Grim

Grim is a simple gem for extracting (reaping) a page from a pdf and converting it to an image as well as extract the text from the page as a string. It basically gives you an easy to use api to ghostscript, imagemagick, and pdftotext specific to this use case.

## Prerequisites

You will need ghostscript, imagemagick, and poppler installed. On the Mac (OSX) I highly recommend using [Homebrew](http://mxcl.github.com/homebrew/) to get them installed.

```bash
$ brew install ghostscript imagemagick poppler
```

## Installation

```bash
$ gem install grim
```

## Usage

```ruby
pdf   = Grim.reap("/path/to/pdf")         # returns Grim::Pdf instance for pdf
count = pdf.count                         # returns the number of pages in the pdf
png   = pdf[3].save('/path/to/image.png') # will return true if page was saved or false if not
text  = pdf[3].text                       # returns text as a String

pdf.each do |page|
  puts page.text
end
```

We also support using other processors (the default is whatever version of Imagemagick/Ghostscript is in your path).

```ruby
# specifying one processor with specific ImageMagick and GhostScript paths
Grim.processor =  Grim::ImageMagickProcessor.new({:imagemagick_path => "/path/to/convert", :ghostscript_path => "/path/to/gs"})

# multiple processors with fallback if first fails, useful if you need multiple versions of convert/gs
Grim.processor = Grim::MultiProcessor.new([
  Grim::ImageMagickProcessor.new({:imagemagick_path => "/path/to/6.7/convert", :ghostscript_path => "/path/to/9.04/gs"}),
  Grim::ImageMagickProcessor.new({:imagemagick_path => "/path/to/6.6/convert", :ghostscript_path => "/path/to/9.02/gs"})
])

pdf = Grim.reap('/path/to/pdf')
```

It is also supported to use another ghostscript executable, which is especially interesting for windows users since the executable is named differently there.

```ruby
# specifying another ghostscript executable, win64 in this example
# the ghostscript/bin folder still has to be in the PATH for this to work
Grim.processor =  Grim::ImageMagickProcessor.new({:ghostscript_path => "gswin64c.exe"})

pdf = Grim.reap('/path/to/pdf')
```

## Reference

* [jonmagic.com: Grim](http://jonmagic.com/blog/archives/2011/09/06/grim/)
* [jonmagic.com: Grim MultiProcessor](http://jonmagic.com/blog/archives/2011/10/06/grim-multiprocessor-to-the-rescue/)

## Contributors

* [@jonmagic](https://github.com/jonmagic)
* [@jnunemaker](https://github.com/jnunemaker)
* [@bryckbost](https://github.com/bryckbost)
* [@bkeepers](https://github.com/bkeepers)
* [@BobaFaux](https://github.com/BobaFaux)
* [@Rubikan](https://github.com/Rubikan)
* [@victormier](https://github.com/victormier)

## License

See [LICENSE](LICENSE) for details.
