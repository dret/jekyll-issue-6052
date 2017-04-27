# jekyll-issue-6052

Demo repo for [jekyll](https://github.com/jekyll/jekyll) [issue #6052](https://github.com/jekyll/jekyll/issues/6052). Visit the GitHub pages version at [`https://dret.github.io/jekyll-issue-6052/`](https://dret.github.io/jekyll-issue-6052/).

* [Link to `md` file](test%2Fescape.md)
* [Link to `json` file](test%2Fescape.json)
* [Link with `.html` extension](test%2Fescape.html)
* [Link with no extension](test%2Fescape)

If you look at the links above, all of them should work, because they refer to resources that jekyll puts in the `_site` directory. However, there are two problems:


## 1. Link rewriting behaves inconsistently

It seems that links rewriting is doing odd and inconsistent things. the above links use the following URIs in the MD source file:

* `test%2Fescape.md`
* `test%2Fescape.json`
* `test%2Fescape.html`
* `test%2Fescape`

However, looking at the HTML source that jekyll produces *on GitHub*, the link targets show up as follows:

* `/jekyll-issue-6052/test%252Fescape.html`
* `/jekyll-issue-6052/test%2Fescape.json`
* `test%2Fescape.html`
* `test%2Fescape`

One problem is that this only happens on GitHub and not on the local jekyll build (where the links are not garbled at all). Notice how GitHub for the first link escapes the escaped URI, for the second link doesn't but still adds the prefix, and only for the last two links seems to do the right thing (leaving the URIs relative and escaped).


## 2. Requests for escaped URIs are not served

Apart from the link rewriting problem that only seems to be broken on GitHub, requests for the resources are not properly served.

* On GitHub pages, only the first link works but it's incorrectly using a doubly escaped URI (which then is served as a doubly escaped resource). The three other links are not working properly and result in 404 responses instead of returning the requested resources.
* On a local jekyll installation, none of the four links work, and they all result in 404 responses (which all report that an error occurred for the *unescaped URIs*, apparently getting the escaping wrong) instead of returning the requested resources.
