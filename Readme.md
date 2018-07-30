# centillion

**the centillion**: a pan-github-markdown-issues-google-docs search engine.

**a centillion**: a very large number consisting of a 1 with 303 zeros after it.

the centillion is 3.03 log-times better than the googol.

## work that is done

* **Stage 1: index folder of markdown files** (done)
    * See [markdown-search](https://git.charlesreid1.com/charlesreid1/markdown-search.git)
    * Successfully using whoosh to index a directory of Markdown files
        * Problem: .git directory cannot be present (or contaminates list of
          indexed directories)
        * Problem: search index stored on disk, not clear how to use on Heroku
        * May need to check in binary search index, or dive headfirst into
          sqlalchemy
        * Not using [pypandoc](https://github.com/bebraw/pypandoc) yet to extract 
          header/emphasis information

Needs work:

* More appropriate schema
* Using more features (weights) plus pandoc filters for schema
* Sqlalchemy (and hey waddya know safari books has it covered)


* **Stage 2: index a repo's github issues** (done)
    * See [issues-search](https://git.charlesreid1.com/charlesreid1/issues-search.git)
    * Successfully using whoosh to index a repository's issues and comments
    * Use PyGithub
    * Main win here is uncovering metadata/linking/presentation issues

Needs work:
- treat comments and issues as separate objects, fill out separate schema fields
- map out and organize how the schema is updated to make it more flexible
- configuration needs to enable user to specify organization+repos

```
"to_index" : {
    "google" : "google-api-python-client",
    "microsoft" : ["TypeCode","api-guidelines"]
}
```


* **Stage 3: index documents in a google drive folder** (done)
    * See [cheeseburger-search](https://git.charlesreid1.com/charlesreid1/cheeseburger-search.git) 
    * Successfully using whoosh to index a Google Drive
        * File names/owners
        * For documents, pandoc to extract content
        * Searchable by document content
    * Use the google drive api (see simple-simon)
    * Main win is more uncovering of metadata issues, identifying
      big-picture issues for centillion


## immediate next steps: fixes

[Components.md](Components.md) - code organization:
- Changing the schema/options feels a bit all over the place
- How can we better integrate everything?
- How can we better split out functionality?

Whoosh:
- fix templates
    - indexed folders thing
    - cut it out!
    - clean up template styles some
- test/figure out integrated schema
    - can we use None for irrelevant field values?
    - jinja template updates?
    - can use boolean values, change display based on that

Licensing:
- need to start from scratch
- unpack markdown functionality
- replace it

Flask routes:
- organizing delta/main index updates
-protecting with github-flask-dance


Stateless
- Use SqlAlchemy to make stateless
- Convert to Heroku-enabled script



# -----------



## how it will work

The centillion uses whoosh, a python library for building
search engines. 

The centillion creates a schema to index our different items,
and then we add documents to it. whoosh requires lots of
configurations of how we want to index each of our items.

The centillion keeps it simple.


### folder of markdown files

The first type of document collection that the centillion
can handle is a folder with markdown files in it.

The centillion will walk the directory and sniff out
markdown files. it will then use pandoc to extract information
from the markdown documents. 

All markdown documents in the folders will be added to
the index.


### github issues

The next type of document collection that the centillion
can handle is a collection of github issues from a 
repository.

The centillion will extract information from the 
issues and comments using PyGithub and use pandoc
to extract information from the markdown in the
issues/comments.

All issues and comments in the repo will be added to
the index.


### google drive

The last type of document collection that the centillion
can handle is a google drive folder. the google drive
API allows the centillion to download documents in multiple
formats (primarily the .docx format), convert to markdown,
and extract information as was done with github
markdown files.

