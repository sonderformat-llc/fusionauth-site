
# Instructions for migrating blog content 

We're migrating from site/_posts to astro/src/content/blog

Steps:

* Claim 5-10 posts here by putting your name in the 'Who is migrating' column: https://docs.google.com/spreadsheets/d/1_0YX-uLRjGYFESBAZoPYeZ-iMKq4Bt7krywg0A1tTok/edit#gid=1303464598 
  * Start with the oldest posts
* Check out `development`. 
* `git pull origin development` to get the latest code.
* Branch it: `git checkout -b mooreds/migrating-blog-1`
* For each of your posts:
  * `git mv` the file from `site/_posts` to `astro/src/content/blog`
    * Give it a new name with a `.mdx` suffix and no date. `2018-09-11-fusionauth-website-how-we-do-it.md` => `fusionauth-website-how-we-do-it.mdx`
  * Update the frontmatter.
    * Replace `excerpt_separator: "<!--more-->"` with `excerpt_separator: "{/* more */}"`
      * most anything could work here but astro will complain about the `<!--more-->` tag specifically, if you have a good reason you can use something else
    * Add a `publish_date`. The value should be the date you removed from the filename.
      * Note: most places use this field to order the shown posts from newest to oldest (index pages, related posts, etc)
    * If needed, add a `updated_date`.
    * `category` changes to `categories`. We now support multiple (space-separated).
    * `author` changes to `authors`. We now support multiple (comma-separated).
    * (optional): add a `featured_tag`. This will govern what is shown in the `More on...` sidebar. If not provided the first of the `tags` will be used.
    * (optional): add a `featured_category`. This will govern what is shown on the `Related Posts` section. If not provided the first of the `categories` will be used.
  * Copy any images, including the header, to a subdirectory under `astro/public/img/blogs`. For older posts you may need to update the references, for newer posts there should already be a subdirectory.
  * Update image reference. Change it from `{% include _image.liquid src="/assets/img/blogs/bootstrap-studio.png" alt="Bootstrap Studio screenshot" class="img-thumbnail float-left mr-md-4" figure=true %}` to `![Bootstrap Studio screenshot.](/img/blogs/fusionauth-website/bootstrap-studio.png)` 
  * Update the "more excerpt" from `<!--more-->` to `{/* more */}`
    * we have a hard-coded cap of 160 characters for the excerpt in most places it is used right now if you forget
    * we exclude any line that starts with `import` at the top of the excerpt so you can add any components you need without worry
  * If there are any `{% raw %}` tags you should remove them, backticks are enough.
  * If there are any callouts, import and use the astro ones instead. More here: https://inversoft.slack.com/archives/C04DGBXKPGC/p1691171143090139
  * If there are any blocks with `{% remote_include` replace them with the [Remote Code](../../components/RemoteCode.astro) component
  * If there are any other liquid tags, you should remove them.
  * Remove references to adding a comment (usually at bottom of post)
  * Check to see how it renders: http://localhost:3000/blog/fusionauth-website-how-we-do-it
  * If there are any filename collisions, add a `-2` to the filename and note it in the `Notes` column.
* On the blog landing page the category callouts and pinned posts are defined [near the top of the file](../../pages/blog/index.astro) (this may move)
* You can add an author's link to personal site (such as Twitter) in the [mappings file](../../pages/blog/mappings.ts) (this may move)
* If you discover any widespread issues, file them here: https://inversoft.slack.com/archives/C04DGBXKPGC/p1691424115389019
* If you have any questions, ask in #documentation-project or add it to the `Notes` column.
* When you are done migrating the 5-10 posts, submit a PR. Open it against `development`.
* Review the PR yourself to make sure you didn't make any errors.
* Review by others TBD?
* Merge the PR to `development`.
* Mark all the posts `Done` in the `Done` column: https://docs.google.com/spreadsheets/d/1_0YX-uLRjGYFESBAZoPYeZ-iMKq4Bt7krywg0A1tTok/edit#gid=1303464598 

## Other notes

* do not check in astro/.astro/types.d.ts