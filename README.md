# Flask-Compress

[![Version](https://img.shields.io/pypi/v/flask-compress.svg)](https://pypi.python.org/pypi/Flask-Compress)
[![Build Status](https://travis-ci.org/libwilliam/flask-compress.png)](https://travis-ci.org/libwilliam/flask-compress)
[![Coverage](https://coveralls.io/repos/libwilliam/flask-compress/badge.svg)](https://coveralls.io/github/libwilliam/flask-compress)
[![License](https://img.shields.io/pypi/l/flask-compress.svg)](https://github.com/libwilliam/flask-compress/blob/master/LICENSE.txt)

Flask-Compress allows you to easily compress your [Flask](http://flask.pocoo.org/) application's responses with gzip.

The preferred solution is to have a server (like [Nginx](http://wiki.nginx.org/Main)) automatically compress the static files for you. If you don't have that option Flask-Compress will solve the problem for you.


## How it works

Flask-Compress both adds the various headers required for a compressed response and gzips the response data. This makes serving gzip compressed static files extremely easy.

Internally, every time a request is made the extension will check if it matches one of the compressible MIME types and will automatically attach the appropriate headers.


## Installation

If you use pip then installation is simply:

```shell
$ pip install flask-compress
```

or, if you want the latest github version:

```shell
$ pip install git+git://github.com/libwilliam/flask-compress.git
```

You can also install Flask-Compress via Easy Install:

```shell
$ easy_install flask-compress
```


## Using Flask-Compress

Flask-Compress is incredibly simple to use. In order to start gzip'ing your Flask application's assets, the first thing to do is let Flask-Compress know about your [`flask.Flask`](http://flask.pocoo.org/docs/latest/api/#flask.Flask) application object.

```python
from flask import Flask
from flask_compress import Compress

app = Flask(__name__)
Compress(app)
```

In many cases, however, one cannot expect a Flask instance to be ready at import time, and a common pattern is to return a Flask instance from within a function only after other configuration details have been taken care of. In these cases, Flask-Compress provides a simple function, `flask_compress.Compress.init_app`, which takes your application as an argument.

```python
from flask import Flask
from flask_compress import Compress

compress = Compress()

def start_app():
	app = Flask(__name__)
    compress.init_app(app)
    return app
```

In terms of automatically compressing your assets using gzip, passing your [`flask.Flask`](http://flask.pocoo.org/docs/latest/api/#flask.Flask) object to the `flask_compress.Compress` object is all that needs to be done.


## Options

Within your Flask application's settings you can provide the following settings to control the behavior of Flask-Compress. None of the settings are required.

| Option | Description | Default |
| ------ | ----------- | ------- |
| `COMPRESS_MIMETYPES` | Set the list of mimetypes to compress here. | `[`<br>`'text/html',`<br>`'text/css',`<br>`'text/xml',`<br>`'application/json',`<br>`'application/javascript'`<br>`]` |
| `COMPRESS_LEVEL` | Specifies the gzip compression level. | `6` |
| `COMPRESS_MIN_SIZE` | Specifies the minimum file size threshold for compressing files. | `500` |
