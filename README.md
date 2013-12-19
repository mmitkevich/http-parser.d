http-parser.D
===

http-parser.D = [joyent/http-parser](https://github.com/joyent/http-parser/) in D programming language.

## Usage

	import std.stdio;
	import http.parser.core;
	...
	auto parser = new HttpParser();
	parser.onMessageBegin = (parser) {
		writeln("Message has just begun");
	};
	parser.onMessageComplete = (parser) {
		writeln("Message has been completed");
	};
	parser.onUrl = (parser, string data) {
		writeln("Url of HTTP message is: ", data);
	};
	parser.onStatusComplete = (parser) {
		writeln("HTTP status is complete");
	};
	parser.onHeader = (parser, HttpHeader header) {
		writeln("Parser Header '", header.name, "' with value '", header.value, "'");
	};
	parser.onBody = (parser, ubyte[] data) {
		writeln("A chunk of the HTTP bady has been processed: ", data);
	};
	parser.execute("GET / HTTP 1.1\r");
	parser.execute("\n");
	parser.execute("FirstHeader: ValueOfFirst Header\r\n");
	parser.execute("Content-Length: 3\r\n");
	parser.execute("\r\n");
	ubyte[] bodyChunk = [1u,2u,3u];
	parser.execute(bodyChunk);


Output:
	Message has just begun
	Url of HTTP message is: /
	Parser Header 'FirstHeader' with value 'ValueOfFirst Header'
	Parser Header 'Content-Length' with value '3'
	A chunk of the HTTP bady has been processed: [1, 2, 3]
	Message has been completed


## Building

    git clone https://github.com/heapsource/http-parser.d.git
	git submodule update --init
	make

An archive will be generated in `out/http-parser.a` containing joyent/http-parser objects and the http-parser.d object itself.

## Examples

Use `make examples` to compile all the examples. Executables will be generated in `out/examples`.


## Test


	make test


## License (MIT)

Copyright (c) 2012, 2013 Heapsource.com - http://www.heapsource.com

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NON-INFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.