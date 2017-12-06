---
layout: post
title: "Markdown Tester"
author: "Nobody"
date: 2015-11-28 10:20:15 +0800
tags: [test, some people, just want to, watch the world burn, by, putting, lots, of, tags]
preview: http://payload345.cargocollective.com/1/0/19261/9211166/random_ani_buns_600x600.gif
---

<!-- # Markdown Tester -->
One Paragraph of `project description` goes here.
Make sure the backtick works.

## JS Codes

```js
  _convertMarkdown(source){
    const markdownSource = source ? source : ""
    const rawMarkup =  marked(markdownSource, {
      highlight: function (code) {
        return hljs.highlightAuto(code).value
      },
      sanitize: false,
    })
    return { __html: rawMarkup }
  }

  document.querySelectorAll('img').forEach(function(img){
  let temp = document.createElement('a');
  temp.href = img.src;
  temp.download = img.src;
  temp.click();
});
```

## HTML
```html
<div class="card qd-card phantom-card">
  <!-- This is placed here to left-align last flex row -->
</div>
<div class="card qd-card phantom-card">
  <!-- This is placed here to left-align last flex row -->
</div>
```

## CSS
```scss
.some-class {
  font-size: 1.2em;
  @media (max-width: $screen-xs) {
    font-size: 0.8em;
  }
}
```

## Ruby Codes

```ruby
# Box drawing class.
class Box
  # Initialize to given size, and filled with spaces.
  def initialize(w,h)
    @wid = w
    @hgt = h
    @fill = ' '
  end

  # Change the fill.
  def fill(f)
    @fill = f
    return self
  end

  # Rotate 90 degrees.
  def flip
    @wid, @hgt = @hgt, @wid
    return self
  end

  # Generate (print) the box
  def gen
    line('+', @wid - 2, '-')
    (@hgt - 2).times { line('|', @wid - 2, @fill) }
    line('+', @wid - 2, '-')
  end

  # For printing
  def to_s
    fill = @fill
    if fill == ' '
      fill = '(spaces)'
    end
    return "Box " + @wid.to_s + "x" + @hgt.to_s + ", filled: " + fill
  end

private
  # Print one line of the box.
  def line(ends, count, fill)
    print ends;
    count.times { print fill }
    print ends, "\n";
  end
end

# Create some boxes.
b1 = Box.new(10, 4)
b2 = Box.new(5,12).fill('$')
b3 = Box.new(3,3).fill('@')
```

### Normal Code Quote

```
Here is a ruby method:
def some_method(name)
  print "Ruby is fun, #{name}!"
end
```

What things you need to install the software and how to install them

```
Give examples
```

### Installing

A step by step series of examples that tell you have to get a development env running

Say what the step will be

```
Give the example
```

And repeat

```
until finished
```

End with an example of getting some data out of the system or using it for a little demo

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags).

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone who's code was used
* Inspiration
* etc

#codestash