# maven-vault-plugin example

This shows how you could push code (and any content really) from your filesystem into a JCR. Storing your work on a filesystem allows you to use source control like git. Doing so has made my life a lot easier, even when working on a project solo.

## Getting started

1. Clone to your machine: `git clone https://github.com/jnraine/maven-vault-plugin-example`.
2. Set local instance in pom.xml (defaults to `http://admin:admin@localhost:4502`).
3. Push code to local instance: `mvn install`.

_Note: You'll need to have [maven](http://maven.apache.org/) installed. It's easy with [Homebrew](http://brew.sh/) (OS X only)._

## Extracting code from JCR

If all your code is in single JCR instance, follow these steps to get it out:

1. Visit `http://localhost:4502/crx/packmgr/index.jsp`, create a package of your code, build it, then download it.
2. Unzip the package and copy `META_INF` and `jcr_root` to the `src` directory.

The package will include all necessary filter and config files for VLT. Feel free to tweak them as necessary (unfortunately, there is not much documentation on what is possible).

## Wait, but how do I edit node properties?

Because the hierarchical structure of the JCR doesn't map directly a typical filesystem, the missing pieces are modelled using XML (or JSON). Nodes are represented as XML files. Nodes with children are represented as a directory with a `.content.xml` file.

For example, take the following JCR structure:

```
apps
|- example
|   |- my_node
|   |- my_other_node
|- foundation
    |- another_node
```

The corresponding filesystem representation would look like this:

```
apps
|- example
|   |- .content.xml
|   |- my_node.xml
|   |- my_other_node.xml
|- foundation
    |- .content.xml
    |- another_node.xml
```

If you wanted to edit a property or the node type of /apps/example, edit apps/example/.content.xml. Take a look at my-node.xml for an example of this.

## A note on push vs pull

Once code is out of the JCR and stored locally, I find it best to edit on the filesystem and push to the JCR instead of via CRXDE Lite.  Pulling code is an involved process and may bring over content you did not intend to commit.