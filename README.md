# DidiWiki-NG 0.5

DidiWiki is a small and simple WikiWikiWeb implementation written in
C. Its intended for personal use for notes, Todo's etc. It includes its
own webserver and hopefully its as simple as just compiling, running
and pointing your browser at it. 

## Installation

To build do the usage `./configure` and `make`. You can then `make install` or
run DidiWiki right from the `src` dir. On startup it'll tell you the address
to point your browser at. You should then be ready to go. 

If upgrading from an earlier version its a good idea to delete
`~/.didiwiki/DidiHelp` before running so you get the latest version of
the help file.

## Remote API

As from 0.5 DidiWiki has a very simple `REST` like http API. Hopefully
this will make it possible to write alternate front ends to didiwiki.

The api looks like this with parameter passable via `GET` or `POST`;

- `http://didiwiki/api/page/get?page=XXX`
Get the raw wiki text for specified page. Returns http 500 code if page does not exist.

- `http://didiwiki/api/page/set?page=XXX&text=XXX`

Set the wikitext of a page. Page is created if it doesn't exist. 

`http://didiwiki/api/page/exists?page=XXX`

Returns `success` if page exists, 500 error if not. 

`http://didiwiki/api/page/delete?page=XXX`

Deletes a page.

`http://didiwiki/api/pages`

Returns a seperated list ( one entry per line containing page title, TAB, modified date) of wiki pages with the most recently modified first.

`http://didiwiki/api/search?expr=XXX`

Like the above but lists only pages which contain `expr`.


## Notes & Excuses

- If you want to contribute, please check the [TODO](TODO)!

- Pages are stored in `~/.didiwiki` (you can override this by setting
`DIDIWIKIHOME` env var to a valid path.). You can add a `styles.css`
to this directory to override the `inbuilt` stylesheet.

- The webserver part is heavily based on cvstrac's internal server,
following the same kind of process;

- Its a pretty simple lightweight forking server. A child process is
forked to handle each http request.

- These forked children are assumed to not be around for very long
(less than a second), thus no real attempt is made to free up
memory in these child processes.

- Its probably not very secure at all. **!!PLEASE DONT RUN AS ROOT!!**

- The code uses `asprintf` in a couple of places. I dont know how portable
this is to non Linux systems. If its a problem to you Feel free to send
patches.

- You can debug segv's by running like `didiwiki debug` which will not
start the http server, it existing in a single process but expecting the
http request on stdin.

- The formatting style is very similar to that of kwiki's - I just like 
kwiki :-)

- I wrote it in C as I've mostly forgotten Python, Perl, etc
(and I wanted it to fit easily on a PDA).
