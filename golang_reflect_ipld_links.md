Using Type Annotation to Construct Object in the Go Language
============================================================

In this article, we are going to look at a package I wrote for the IPFS project that sole purpose is to parse a JSON compatible data structure and extract links from it. Because a link can be many things, we can't just have a single `Link` type. Some links have a size attribute. Other links can have permissions attached. In the end, there is an endless flow of possibilities.

In order to accomodate this need for extensibility, we are going to use struct field annotation in Go, then use these annotation to fill in the new objects the values we extract from the data structure.

But first, what exactly are IPFS and IPLD?

IPLD: the new storage backend for IPFS
--------------------------------------

IPFS is a new protocol that changes radically how we view the Web. One of the major problems with the Web is that every single page is owned by someone. When this someone is a big corporation, it can spy on us easily. When that someone goes out of buisness, stops paying servers or dies, the page is no longer accessible. And there is no way to backup the web as most of the pages relies on special code running on the server. In the old times that was limited to CGI scripts, now it is the web application backends. Most sites won't work without this server side component that is always very specific and not accessible to the users.

In opposition to that, IPFS core principle is to address the content itself and not the server it runs on. Instead of asking some specific blog post to a computer identified as `wordpress.com`, you will be asking the whole comunity of IPFS servers for the specific post you want to look at.

If the original author of this post is no longer maintaining his blog, as long as a IPFS enabled computer is interested in keeping a copy, you can still access the post.

**IPFS makes the web permanent.** No more dead links and 404 errors.

This comes with a cost: it is no longer possible to write custom server side scripts. When you publish your documents, it is not possible to customize it for each user. This draws a clear line between web sites, that expose mostly static information, and web application, that need custom processing for each and every action. This moves back the applications to the client that will have to manage everything as there is no server to do that on the users behalf. **IPFS frees the user from the server.**