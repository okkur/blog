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

Syna is a theme for Hugo static website generator. While most themes have predefined layouts for each page and configuration is limited, Syna allows you to create and modify the layout of each page with building blocks we call fragments. There are tons of fragments already built in and you can create your own fragments using the structure provided by Syna. We will discuss this is another article.

Syna can be used to create company websites, pages to introduce products, display charts, blogs and so much more. In this tutorial we're gonna discuss the creation of a blog.

You can create the base structure for a Hugo website using the Getting Started guide. You can also clone the syna-start repository (you need to have already installed [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), [Go](https://golang.org/doc/install/source) and [Hugo](https://gohugo.io/getting-started/installing/)):

```bash
git clone https://git.okkur.org/syna-start syna-blog
cd syna-blog
git submodule init
git submodule update
hugo server -D
```

Now you can open [http://localhost:1313](http://localhost:1313) and check out the sample website we just cloned. Take your time and check out the folder structure and run the server to browse the website. There are two pages in the repository, index and about. Each page has some fragments already displayed in them. You'll notice some fragments are the same. These fragments (nav, footer, copyright) are defined globally by their files in `content/_global` directory. If you want you can open `themes/syna/exampleSite` to check out our main website in depth.

In order to create our blog, we need to clean up the sample project we just cloned and create the base files.

```bash
mkdir content/blog # our blog section
touch content/blog/_index.md # blog's main page
```

The `blog/_index.md` file is a "list" page in Hugo. If you're unfamiliar with how Hugo manages content please check out the [quick start](https://gohugo.io/getting-started/quick-start/) page of their documentation. A list page is configured by the `_index` directory in the same directory as the `_index.md` file. We will configure this page later on. Let's create our first blog post.

```bash
mkdir content/blog/hello-world
touch content/blog/hello-world/index.md
touch content/blog/hello-world/content.md
```

You can create pages directly in the blog directory but that would prevent us from configuring the page's fragments. So we create a directory for each post and adding a [`content` fragment](https://about.okkur.org/syna/fragments/content/) for each one. `content` fragment is responsible for showing content in an article format. Add the following to the `content.md` file:

```markdown
+++
fragment = "content"
weight = 100
+++

Hello world! This is my first blog post.
```

Hugo's content files are markdown files with [FrontMatter](https://gohugo.io/content-management/front-matter/) supporrt. FrontMatter (the part on top between the two `+++`) is used to configure the content and the markdown that comes after is usually the content that is shown on the page. In most of our built-in fragments we only use the FrontMatter configuration and only use markdown for content in the `content` fragment and related functionality.

The code above will create our `content` fragment, sorting it by it's weight among other pages and global fragments and display the content that is written after the FrontMatter. You can read more about what each variable does and how each fragment works in [Syna's official documentation](https://about.okkur.org/syna/docs/).

Now that we have our first blog post, we need to make Hugo recognize it as a page. So we're gonna fill in `content/blog/hello-world/index.md` file we previously created:

```markdown
+++
title = "Hello World!"
date = "2019-11-01"
+++
```

Now if you start the development server (`hugo server -D`) and visit [https://localhost:1313/blog/hello-world](https://localhost:1313/blog/hello-world) you can see the blog post. You can add more [fragments](https://about.okkur.org/syna/fragments/) to the page to customize how it looks and functions. But there isn't yet any way to view the archive of the blog and there isn't any way to access this page without the url. We need to add the list of the blog pages in the blog "list" page located at [https://localhost:1313/blog](https://localhost:1313/blog). In order to do so, create a new directory in `content/blog` by running the following commands:

```bash
mkdir content/blog/_index
mkdir content/blog/_index/index.md
```

Hugo recognizes each directory with `index.md` or `_index.md` inside it as a page, but since we're gonna use this directory as configuration center for our list page and not to create a different page, we need to force Hugo to not create a page for it. Add the following to `content/blog/_index/index.md`:

```markdown
+++
headless = true
+++
```

## Display an archive of the blog
Now every file in this directory will be displayed in the list page of our blog section. We can add a [`list` fragment](https://about.okkur.org/syna/fragments/list/) to display the latest posts. Create a `list.md` file inside our `_index` directory and add the following to it:

```markdown
+++
fragment = "list"
weight = 100

section = "blog"
```

Now go ahead and visit [https://localhost:1313/blog](https://localhost:1313/blog). You can also duplicate this file and move it to `content/_index` to display latest blog posts in your home page.

## Deploy

You can easily deploy a Syna website on Netlify. Just create a reposity on Github or Gitlab and connect it to your Netlify account.

We will discuss adding more info to your pages in later posts.
