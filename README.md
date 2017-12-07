## Usage

### Installation

    bundle install
    

### Development

Run Jekyll:

    bundle exec jekyll serve


### Write a Blog Post

1. Create a new file in `_posts` folder.

2. Filename should be in the following format:
    `YEAR-MONTH-DAY-title.md`
    eg: `2016-11-22-next-academy-is-awesome.md`

3. Posts should start with:
    ```
    ---
    layout: post
    title: "Sample Post" <!-- Provide appropriate post title -->
    author: "John Doe" <!-- Your name -->
    date: 2016-11-22 12:31:14 +0800 <!-- DateTime with +0800 timezone -->
    tags: [ruby, elixir] <!-- appropriate tags -->
    preview: http://image-related-to/your-post.png
    ---
    ```

4. Every post **_must_** have a related preview image. Make sure the image is at least _270x250_, and close to 1:1 aspect ratio (squarish image).

5. Other resources:
    - Jekyll Docs on [Writing Posts](https://jekyllrb.com/docs/posts/)
    - Jekyll Docs on [Syntax Highlighting](https://jekyllrb.com/docs/posts/#highlighting-code-snippets)

## License

MIT License
