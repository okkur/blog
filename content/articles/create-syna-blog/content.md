+++
fragment = "content"
date = "2019-12-05"
weight = 10

title = "Create a blog using Hugo and Syna"
title_align = "left"

display_date = true

[asset]
  image = "typewriter.jpg"
  text = "Typewriter"
+++

Syna is a theme for the Hugo static website generator. While most themes have a limited set of configurations and predefined layouts for each individual page, Syna allows you huge flexibility by spliting each page up into building blocks we call fragments. There are tons of fragments already built in and you can create your own fragment using Syna as a framework.

Syna can be used to create company websites, introduce products, display charts, blogs and so much more.
In this tutorial we're gonna discuss the creation of a blog.

You can create the base structure for a Hugo website by cloning our starter repository. Prerequisites you need installed are [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), [Go](https://golang.org/doc/install/source) and [Hugo](https://gohugo.io/getting-started/installing/).

Step by step:  
```bash
git clone --recursive https://git.okkur.org/syna-start my-blog
cd my-blog
hugo server -D
```

Now you can open [http://localhost:1313](http://localhost:1313) and check out the sample website we just cloned. Take your time and check out the folder structure and browse the website. There are two pages being created based on the starter repository, an index/main page and an about page. Each one has some fragments already being displayed. You'll notice some fragments are the same in both pages. These fragments (nav, footer, copyright) are defined globally and are shared by all pages, if not overwritten. These global fragments are defined within the `content/_global` directory.

In order to create our blog we need to add a blog directory for our section as well a few files:

```bash
mkdir content/blog # our new blog section
touch content/blog/_index.md # our blog's skeleton index/main page used for setting the title and description
```

The `blog/_index.md` declares the whole directory a section with child pages to Hugo. For more details on how Hugo manages content please check out the [quick start](https://gohugo.io/getting-started/quick-start/) page of their documentation.

Let's first create our first blog post:

```bash
mkdir content/blog/hello-world # directory for our new blogpost will be served as a single page
touch content/blog/hello-world/index.md # declaring the title and date of the whole page
touch content/blog/hello-world/content.md # our content as a content fragment
```

We create a directory for each post and add a [`content` fragment](https://about.okkur.org/syna/fragments/content/) for each one. The `content` fragment is responsible for showing the content in an article format.
You can create pages directly in the blog directory without the additional subdirectory, but that would prevent us from configuring additional page fragments.

To define the content add the following to the `content/blog/hello-world/content.md` file:

```
+++
fragment = "content"
weight = 100
+++

Hello world! This is my first blog post.
```

Hugo's content files are markdown files with [FrontMatter](https://gohugo.io/content-management/front-matter/) support. FrontMatter (the part on top between the two `+++`) is used to configure the fragment. The text, usually in markdown format, that comes after will be the content that is shown on the page. Most of our built-in fragments only use the FrontMatter configuration.

The code will create our `content` fragment, sorting it by its weight among other pages and global fragments and display the content that is written after the FrontMatter. You can read more about each variable and how each fragment works in [Syna's official documentation](https://about.okkur.org/syna/docs/).

Now that we have our first blog post content, we can let Hugo recognize it as a page. We're gonna fill the `content/blog/hello-world/index.md` file we previously created with the title and a date for the page and declare the directory to be a single page due to being named `index.md`:

```markdown
+++
title = "Hello World!"
date = "2019-11-01"
+++
```

Now if you start the development server (`hugo server -D`) and visit [https://localhost:1313/blog/hello-world](https://localhost:1313/blog/hello-world) directly you can see our first blog post. You can add more [fragments](https://about.okkur.org/syna/fragments/) to the page to customize how it looks and functions.

Currently we haven't yet configured any way to show a list of all posts. For this we can create a `list` page.
In order to do so, we create a new directory in `content/blog` by running the following commands:

```bash
mkdir content/blog/_index
touch content/blog/_index/index.md
```

Syna has two special use directories so called headless pages. We use the `_index` and `_global` directory to configure a sections main/index page or add global fragments. To make this function correctly we need to tell Hugo to not create a page, but instead declare them as a headless directory. This can be done by adding the following to `content/blog/_index/index.md`:

```markdown
+++
headless = true
+++
```

Now we are going to add a [`list` fragment](https://about.okkur.org/syna/fragments/list/) to our blog section page. Currently it is an empty page with our global fragments showing, but no posts listed.
Create a `list.md` file inside our `content/blog/_index` directory and add the following:

```markdown
+++
fragment = "list" # declare list fragment
weight = 100

section = "blog" # section to pull the list of pages
```

Now go ahead and visit [https://localhost:1313/blog](https://localhost:1313/blog). You can also duplicate this file and move it to `content/_index` to display latest blog posts in your home page as well.


You can easily deploy a Syna website on Netlify. Create a reposity on Github or Gitlab and connect it to your Netlify account.
For more options on how to deploy a site take a look at the (Hugo documentation](https://gohugo.io/hosting-and-deployment/).

Let us know what you build on [Twitter](https://twitter.com/okkurlabs) or give feedback and contribute on [Github](https://syna.okkur.org/code).

Happy building from the Okkur Team