Introduction to IPFS
====================

IPFS is a new protocol that changes radically how we view the Web. One of the major problems with the Web is that every single page is owned by someone. When this someone is a big corporation, it can spy on us easily. When that someone goes out of buisness, stops paying servers or dies, the page is no longer accessible. And there is no way to backup the web as most of the pages relies on special code running on the server. In the old times that was limited to CGI scripts, now it is the web application backends. Most sites won't work without this server side component that is always very specific and not accessible to the users.

In opposition to that, IPFS core principle is to address the content itself and not the server it runs on. Instead of asking some specific blog post to a computer identified as `wordpress.com`, you will be asking the whole comunity of IPFS servers for the specific post you want to look at.

If the original author of this post is no longer maintaining his blog, as long as a IPFS enabled computer is interested in keeping a copy, you can still access the post.

**IPFS makes the web permanent.** No more dead links and 404 errors.

This comes with a cost: it is no longer possible to write custom server side scripts. When you publish your documents, it is not possible to customize it for each user. This draws a clear line between web sites, that expose mostly static information, and web application, that need custom processing for each and every action. This moves back the applications to the client that will have to manage everything as there is no server to do that on the users behalf. **IPFS frees the user from the server.**

Because IPFS addresses content and not web servers, it is more difficult to update an already published document. Once the document is published, it will always be available unmodified. Modified versions will be addressed using other identifiers, making it difficult to discover new versions of the documents. To solve this problem, instead of addressing document directly, it is possible to address content producers. These are identified by a public cryptographic key and they can sign new versions of their documents. Lookup of the public key allows fetching all versions of a document, including the latest version, with the added assurance that the document is authentic. **IPFS makes the web secure.**

The technical details: the *merkle graph*
-----------------------------------------

Documents are in fact stored as *merkle-trees*. This is the same principle used by the blockchain or Git. It is a directed acyclic graph where the links between each node of the graph is a cryptographic hash. Once you hold the root of the tree, you can be sure that all the nodes reachable using this root are still exactly the way they were when the tree was published.

Each node consist of both data and links to child nodes. For a long time, this was just as simple. Now IPFS is moving towards the IPLD backend that allows to express more in each node.

The anatomy of a node: the IPLD format
--------------------------------------

IPLD stands for *IPFS Linked Data* and is inspired from JSON-LD that add a structure, including links, to JSON documents. IPLD is compatible with both JSON and JSON-LD because the underlying data model is the same.

Basically, an IPLD document can be constructed from arrays, maps (JSON objects), or simple values like booleans, integers, textual or binary strings. Instead of reusing the JSON-LD link model which requires much overhead like a schema and a context, IPLD uses a simple notation. An IPLD node, converted to YAML, could look like this:

    ---
    title: "Introduction to IPFS"
    content: "IPFS is a new protocol that..."
    references:
      jsonld:
        @link: /path/to/jsonld/spec
        rel: related
    comments:
      - @link: /path/to/first/comment
        author: Somebody
        content: "This is a very nice introduction ..."
        rel: reply
      - @link: /path/to/second/comment
        author: Somebody
        content: "I would like to add ..."
        rel: reply

Here you can see the document can contain structured data andd that links are objects having a `@link` key pointing to the target document. Links can also have properties that can be a cached version of some elements of the target.

The link paths, in their simplest form, can just be the hash of the target document. Paths are introduced so it is possible to reference sub section of another document, or refer to documents using other addressing schemes.

For example:

- `/ipfs/QmS7zrNSHEt5GpcaKrwdbnv1nckBreUxWnLaV4qivjaNr3` addresses the document hashing to `QmS7z...Nr3`
- `/ipfs/QmS7zrNSHEt5GpcaKrwdbnv1nckBreUxWnLaV4qivjaNr3/comments` addresses the `comments` section of the same document
- `/ipns/QmPCuqUTNb21VDqtp5b8VsNzKEMtUsZCCVsEUBrjhERRSR` addresses a document using the IPNS addressing scheme

Making the documents mutable: the records
-----------------------------------------