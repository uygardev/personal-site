# My Personal Blog

This is the readme file for my personal blog created using the Hugo framework and the Doit template. The design has been customized by me to give it a look similar to that of the Medium.

## Table of Contents

- [Description](#description)
- [Installation](#installation)
- [Usage](#usage)

## Description

My personal blog is built using the Hugo framework, which is a fast and modern static site generator written in Go. The design of the blog is based on the Doit template, which I have customized to give it a look similar to that of the Medium.

The blog features articles on a wide range of topics, including technology and personal development. The articles are written by me and are meant to provide useful insights and tips to my readers.

## Installation

To run this blog on your local machine, you need to have Hugo installed. Here are the steps to install Hugo:

1. Go to the [official Hugo website](https://gohugo.io/) and download the latest version of Hugo for your operating system.
2. Install Hugo by following the instructions provided in the documentation.

Once you have Hugo installed, you can clone this repository to your local machine by running the following command:

```bash
git clone https://github.com/uygardev/personal-site.git
```

## Usage

To run the blog on your local machine, navigate to the root directory of the blog in your terminal and run the following command:

```bash
hugo server
```

This will start a local server that you can access in your web browser by going to [http://localhost:1313](http://localhost:1313).

To create a new post, run the following command in your terminal:

```bash
hugo new posts/your-post-title.md
```

This will create a new markdown file in the `content/posts` directory with the specified title. You can then open the file in your text editor and start writing your post.

Once you are done writing your post, you can preview it by running the `hugo server` command again and navigating to [http://localhost:1313/your-post-title](http://localhost:1313/your-post-title).

To deploy your blog to a live website, you can follow the instructions provided in the Hugo documentation.