Today I Learned @ NEXT Academy is inspired by [Today I Learned](https://til.hashrocket.com/).
Powered by [Jekyll](https://jekyllrb.com/).

## Usage

### Installation

    bundle install

### Write a Post

*IMPORTANT* Check out to a new branch and raise a PR when you're done.

Posts should be limited to approx. 200 words. Keep it short and concise.

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
    ---
    ```

4. When you add tags, check `_tags` folder to make sure `<your-tag>.md` exists.
    If not, create `<your-tag>.md` file with:
    ```
    ---
    slug: elixir
    name: Elixir
    ---
    ```

5. Other resources:
    - Jekyll Docs on [Writing Posts](https://jekyllrb.com/docs/posts/)
    - Jekyll Docs on [Syntax Highlighting](https://jekyllrb.com/docs/posts/#highlighting-code-snippets)

### Development

Run Jekyll:

    bundle exec jekyll serve

## License

MIT License
