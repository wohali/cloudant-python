# Divan [![Build Status](https://travis-ci.org/garbados/divan.png)](https://travis-ci.org/garbados/divan) [![Coverage Status](https://coveralls.io/repos/garbados/divan/badge.png)](https://coveralls.io/r/garbados/divan)

[wiki]: http://en.wikipedia.org/wiki/Divan_(furniture\)
[wiki_img]: http://upload.wikimedia.org/wikipedia/commons/e/ea/FrancisLevettLiotard.jpg

An effortless Cloudant interface for Python.

Put on your favorite hookah, sit back on the [divan][wiki], and relax.

![What a Divan Looks Like][wiki_img]

## Install

    pip install divan
    
## Usage

All methods return response objects generated by Requests, if the request was successful. Else, the response object is attached to the thrown error. If a response has a bad status code, Divan throws an error.

Divan expose raw interactions -- HTTP requests, etc. -- through special methods, so we provide syntactical sugar without obscuring the underlying API. (builtins, such as `__getitem__`, act as Pythonic shortcuts to those methods)

If CouchDB has a special endpoint for something, it's in Divan as a special method, so any special circumstances are taken care of automagically.

You can create objects explicitly or inherit them from objects higher in the DB hierarchy. So, you can create a `Database` object explicitly, or use `Connection.database` to inherit settings from the `Connection` object.

## API

### Resource(uri, **kwargs)

REST resource class: implements GET, POST, PUT, DELETE methods
for all other Divan objects, and manages settings inheritance.

If you create an object, like a `Connection`, then use that to
create a `Database` object, the `Database` will inherit any options
from the `Connection` object, like 

Implements CRUD operations for all other Divan objects.

#### Resource.put(path, **kwargs)

Make a PUT request against the object's URI joined
with `path`.

`kwargs['params']` are turned into JSON before being
passed to Requests. If you want to indicate the message
body without it being modified, use `kwargs['data']`.

#### Resource.post(path, **kwargs)

Make a POST request against the object's URI joined
with `path`.

`kwargs['params']` are turned into JSON before being
passed to Requests. If you want to indicate the message
body without it being modified, use `kwargs['data']`.

#### Resource.get(path, **kwargs)

Make a GET request against the object's URI joined
with `path`. `kwargs` are passed directly to Requests.

#### Resource.delete(path, **kwargs)

Make a DELETE request against the object's URI joined
with `path`. `kwargs` are passed directly to Requests.

### Connection(uri, **kwargs)

#### Connection.info(**kwargs)

Return information about your CouchDB / Cloudant instance.

#### Connection.all_dbs(**kwargs)

List all databases.

#### Connection.get(path, **kwargs)

Make a GET request against the object's URI joined
with `path`. `kwargs` are passed directly to Requests.

#### Connection.database(name, **kwargs)

Create a `Database` object prefixed with this connection's URL.

#### Connection.uuids(count, **kwargs)

Generate an arbitrary number of UUIDs.

#### Connection.session(**kwargs)

Get current user's authentication and authorization status.

#### Connection.logout(**kwargs)

De-authenticate the connection's cookie.

#### Connection.replicate(source, target, opts, **kwargs)

Begin a replication job.
`opts` contains replication options such as whether the replication
should create the target (`create_target`) or whether the replication
is continuous (`continuous`).

Note: unless continuous, will not return until the job is finished.

#### Connection.put(path, **kwargs)

Make a PUT request against the object's URI joined
with `path`.

`kwargs['params']` are turned into JSON before being
passed to Requests. If you want to indicate the message
body without it being modified, use `kwargs['data']`.

#### Connection.active_tasks(**kwargs)

List replication, compaction, and indexer tasks currently running.

#### Connection.login(username, password, **kwargs)

Authenticate the connection via cookie.

#### Connection.post(path, **kwargs)

Make a POST request against the object's URI joined
with `path`.

`kwargs['params']` are turned into JSON before being
passed to Requests. If you want to indicate the message
body without it being modified, use `kwargs['data']`.

#### Connection.delete(path, **kwargs)

Make a DELETE request against the object's URI joined
with `path`. `kwargs` are passed directly to Requests.

### Database(uri, **kwargs)

Connection to a specific database

#### Database.all_docs(**kwargs)

Return an iterator over all documents in the database.

#### Database.get(path, **kwargs)

Make a GET request against the object's URI joined
with `path`. `kwargs` are passed directly to Requests.

#### Database.view_cleanup(**kwargs)

#### Database.revs_diff(revs, **kwargs)

#### Database.save_docs(docs, **kwargs)

Save many docs, all at once.

#### Database.missing_revs(revs, **kwargs)

#### Database.put(path, **kwargs)

Make a PUT request against the object's URI joined
with `path`.

`kwargs['params']` are turned into JSON before being
passed to Requests. If you want to indicate the message
body without it being modified, use `kwargs['data']`.

#### Database.post(path, **kwargs)

Make a POST request against the object's URI joined
with `path`.

`kwargs['params']` are turned into JSON before being
passed to Requests. If you want to indicate the message
body without it being modified, use `kwargs['data']`.

#### Database.document(name, **kwargs)

Create a `Document` object from `name`.

#### Database.changes(**kwargs)

Gets a list of the changes made to the database. This can be used to monitor for update and modifications to the database for post processing or synchronization.

Automatically adjusts the request to handle the different response behavior of polling, longpolling, and continuous modes.

#### Database.delete(path, **kwargs)

Make a DELETE request against the object's URI joined
with `path`. `kwargs` are passed directly to Requests.

### Document(uri, **kwargs)

#### Document.get(path, **kwargs)

Make a GET request against the object's URI joined
with `path`. `kwargs` are passed directly to Requests.

#### Document.merge(docname, change, **kwargs)

Get document by `docname`, merge `changes`,
and then `PUT` the updated document back to the server

#### Document.attachment(name, **kwargs)

Create an `Attachment` object from `name` and the settings
for the current database.

#### Document.put(path, **kwargs)

Make a PUT request against the object's URI joined
with `path`.

`kwargs['params']` are turned into JSON before being
passed to Requests. If you want to indicate the message
body without it being modified, use `kwargs['data']`.

#### Document.post(path, **kwargs)

Make a POST request against the object's URI joined
with `path`.

`kwargs['params']` are turned into JSON before being
passed to Requests. If you want to indicate the message
body without it being modified, use `kwargs['data']`.

#### Document.view(method, function, **kwargs)

Create a `View` object by joining `method` and `function`.

#### Document.delete(path, **kwargs)

Make a DELETE request against the object's URI joined
with `path`. `kwargs` are passed directly to Requests.

### View(uri, **kwargs)

Methods for design documents

#### View.put(path, **kwargs)

Make a PUT request against the object's URI joined
with `path`.

`kwargs['params']` are turned into JSON before being
passed to Requests. If you want to indicate the message
body without it being modified, use `kwargs['data']`.

#### View.post(path, **kwargs)

Make a POST request against the object's URI joined
with `path`.

`kwargs['params']` are turned into JSON before being
passed to Requests. If you want to indicate the message
body without it being modified, use `kwargs['data']`.

#### View.get(path, **kwargs)

Make a GET request against the object's URI joined
with `path`. `kwargs` are passed directly to Requests.

#### View.delete(path, **kwargs)

Make a DELETE request against the object's URI joined
with `path`. `kwargs` are passed directly to Requests.

### Attachment(uri, **kwargs)

Attachment methods for a single document

#### Attachment.put(path, **kwargs)

Make a PUT request against the object's URI joined
with `path`.

`kwargs['params']` are turned into JSON before being
passed to Requests. If you want to indicate the message
body without it being modified, use `kwargs['data']`.

#### Attachment.post(path, **kwargs)

Make a POST request against the object's URI joined
with `path`.

`kwargs['params']` are turned into JSON before being
passed to Requests. If you want to indicate the message
body without it being modified, use `kwargs['data']`.

#### Attachment.get(path, **kwargs)

Make a GET request against the object's URI joined
with `path`. `kwargs` are passed directly to Requests.

#### Attachment.delete(path, **kwargs)

Make a DELETE request against the object's URI joined
with `path`. `kwargs` are passed directly to Requests.


## Testing

To run Divan's tests, just do:

    python setup.py test

## Documentation

The API reference is automatically generated from the docstrings of each class and its methods. To install Divan with the necessary extensions to build the docs, do this:

    pip install -e divan[docs]

Then, in Divan's root directory, do this:
  
    python docs

Note: docstrings are in [Markdown](http://daringfireball.net/projects/markdown/).