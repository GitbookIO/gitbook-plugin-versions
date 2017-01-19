## gitbook-plugin-versions

Display a `<select>` with other versions of your gitbook.

### Usage with gitbook.com

When your book is hosted on [GitBook.com](https://www.gitbook.com), the plugin can access a listing of versions using the [GitBook API](http://developer.gitbook.com/books/versions/).

Put this in your book.json:

```js
{
    "plugins": [ "versions" ],
    "pluginsConfig": {
        "versions": {
            "type": "branches"
        }
    }
}
```

Type can have different values:

- Branches (`branches`)
- Tags (`tags`)
- Languages (`languages`)

### Usage with a manual configuration

Put this in your book.json:

```js
{
    "plugins": [ "versions" ],
    "pluginsConfig": {
        "versions": {
            "gitbookConfigURL": "https://raw.githubusercontent.com/rackt/history/gh-pages/book.json",
            "options": [
                {
                    "value": "https://rackt.github.io/history/v1.3.0/",
                    "text": "Version 1.3.0"
                },
                {
                    "value": "https://rackt.github.io/history/v1.4.0/",
                    "text": "Version 1.4.0",
                    "selected": true
                }
            ]
        }
    }
}
```

A `<select>` element will be created with the given `options` and placed at the top of the book summary. When the user selects one of the options, they are taken to that URL.

The `gitbookConfigURL` variable is a publicly accessible URL to your `book.json`. If this is present, the plugin will fetch the latest config when the page loads, so even older versions of your book will have updated `options`.
However, each version of your book needs to have book.json, which defines the `gitbookConfigURL` variable. It should also define at least an option for the current version, with `selected` set to true. 
The list of options will be overwritten by the config in `gitbookConfigURL`, but the current selected version will be matched according the value (URL) of the previous selected option.

#### An example how to define 2 versions with a common config

Let's have 2 versions in branches v1 and v2. We want to define a common config in another branch/repository, so that we need to update only single config file if we add new versions later.

1. Create a completely new branch (can be empty) or a new empty repository
2. Create a `book.json` file in the new config branch/repo with configuration for `versions` plugin in `pluginsConfig`. Define options with value and text for v1 and v2.
3. Create a `book.json` file in v1 and v2 branches as in the above example. Add `gitbookConfigURL` pointing to raw book.json file in your config branch
4. For both v1 and v2 branches, add single element into `options` in book.json - the`value` should point to the gitbooks URL matching the v1 or v2 branch, the `text` is the label s defined in the conig branch, and `selected` should be set to true

You will end up with 3 `book.json` files in 3 branches. Branch v1 contains a single option pointing to the v1 documentation, v2 contains again a single option pointing to the v2 documentation. Both also contain referece to the common book.json config in another branch. 
The book.json file in the config branch is the only file you need to modify from now on. 

Let's add a new version:

1. Create v3 branch (based on v2 branch)
2. Modify `book.json` acordingly - update the value with correct v3 URL and text (label)
3. Modify `book.json` in config branch - copy the option from v3 branch (after modifications) and add it to the list of options

And you are done. All 3 options will be available in all 3 versions of the documentation.

### Credits

Original work by [@mjackson](https://github.com/mjackson).
