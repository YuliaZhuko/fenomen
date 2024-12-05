---
title: First Blog Post
slug: first-blog-post
tags:
  - foo
  - bar
authors:
  - name: Garrison McMullen
    title: Instruction Writer
    url: https://github.com/garrison0
    image_url: https://avatars.githubusercontent.com/u/4089393?v=4
---
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque elementum dignissim ultricies. Fusce rhoncus ipsum tempor eros aliquam consequat.
Procedure
Create an admin directory inside static.
cd static
mkdir admin
In the admin directory, create a config.yml file and an index.html file.
cd admin
touch config.yml
touch index.html
Edit index.html to look like this:
<!doctype html>
<html>
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Content Manager</title>
</head>
<body>
  <!-- Include the script that builds the page and powers Decap CMS -->
  <script src="https://unpkg.com/decap-cms@^3.0.0/dist/decap-cms.js"></script>
</body>
</html>
index.html displays the Decap CMS admin interface. You'll use the admin interface to edit your blog posts.

Edit config.yml to look like this:
backend:
name: github
branch: main
repo: <your-github>/my-website

# These lines should *not* be indented
media_folder: "static/img" # Media files will be stored in the repo under static/images/uploads
public_folder: "/img/" # The src attribute for uploaded media will begin with /images/uploads

collections:
- name: blog
  label: "blog"
  folder: blog
  identifier_field: title
  extension: md
  widget: "list"
  create: true
  slug: "{{year}}-{{month}}-{{day}}-{{slug}}" # Filename template, e.g., YYYY-MM-DD-title.md
  fields:
    - { name: title, label: Title, widget: string }
    - { name: body, label: Body, widget: markdown }
    - { name: slug, label: Slug, widget: string }
    - label: "Tags"
      name: "tags"
      widget: "list"
    - label: "Authors"
      name: "authors"
      widget: "list"
      fields:
        - { name: name, label: Name, widget: string }
        - { name: title, label: Title, widget: string }
        - { name: url, label: URL, widget: string }
        - { name: imageUrl, label: ImageURL, widget: string }
          config.yml specifies what kind of content your blog posts have. The content specification enables Decap CMS to edit existing posts and create new ones with the same format. To learn more, read about Decap CMS' Configuration options.

Visit localhost:3000/admin
You can now view and edit 2021-11-15-first-blog-post.md through the admin interface. You can also create new blog posts.

Warning: Any changes you publish through the admin interface will only effect your remote GitHub repository. To retrieve these changes locally, git pull from your local repository.

Commit and push your new changes to your remote repository.
git add .
git commit -m "Add Decap CMS"
git push
Netlify builds and deploys your new changes.

Add GitHub as an authentication provider
Before you can access /admin/ through your Netlify domain, you need to set up an authentication provider. The authentication provider allows Decap CMS to determine whether users have read and write access to /admin/. This guide uses GitHub credentials for authentication.

Configure GitHub
Create a new GitHub OAuth application.
Enter your Netlify domain as the Homepage URL.
Enter https://api.netlify.com/auth/done as the Authorization callback URL.
Click Register application.
Click Generate a new client secret.
Copy the provided client secret and client ID.
Configure Netlify
On Netlify, under Site configuration > Access & security > OAuth > Authentication providers, click Install provider.
Enter your client secret and client ID from GitHub.
Click Install.
🎉 All done! Now you can access the admin interface through your Netlify URL.

