# What it is

Node.js bindings for [libxl](http://www.libxl.com/).

# How to use it

## Installation

Pull the library into your project with `npm install libxl`
and require the module via

    var xl = require('libxl');

## API

A new excel document is created via

    var xlsBook = new xl.Book(xl.BOOK_TYPE_XLS);

or

    var xlsxBook = new xl.Book(xl.BOOK_TYPE_XLSX);

(for xlsx documents). The document is written to disk via

    xlsBook.writeSync('file.xls');

(loading and async writing are not implemented atm).

The API closely follows the
[libxl documentation](http://www.libxl.com/documentation.html).
For example, adding a new sheet and writing two cells works as

    var sheet = xlsBook.addSheet('Sheet 1');
    sheet.writeStr(1, 0, 'A string');
    sheet.writeNum(1, 1, 42);

Functions whose C++ counterpart returns void or an error status
have been implemented to return the respective instance, so it
is possible to chain calls

    sheet
        .writeStr(1, 0, 'A string');
        .writeNum(1, 1, 42);

Errors are handled by throwing exceptions.

API coverage is a work in progress; see demo.js,
jasmine specs and the class initializers
(Book::Initialize, Sheet::Initialize, Format::Initialize)
to find out what is currently supported :).

## Unlocking the API

If you have purchased a licence key from XLware, you can
build it into the bindings by modifying `api_key.h` and
rebuilding the library via `node-gyp rebuild` (you'll have
to install node-gyp for this) or `npm install` in the package
directory. I might implement `book.setKey` at some point in the
future, too.

## Platform support

The package currently supports Linux, Windows and Mac.

# How to contribute

I'll be happy to merge in all sensible pull requests. If you
implement a new API method, please add a short test to the
jasmine specs and a call to demo.js (if applicable).

# Credits

* Torben Fitschen wrote the install script which pulls the
  necessary libxl SDK before building.
* Alexander Makarenko wrote
  [node-excel-libxl](https://github.com/7eggs/node-excel-libxl)
  Though node-libxl is rewritten from scratch, this
  package served as the starting point.
