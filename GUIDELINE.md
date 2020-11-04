# Guidelines to post in EKbanaML study group

## A: User

### 1. Add User
Add details to: `/_data/authors.yml`

                your-username:
                        name    : "your-Full-Name"
                        bio     : "your-bio"
                        avatar  : "/assets/images/userPhoto/no_photo.jpg"
                        links:
                        - label: "Email"
                        icon: "fas fa-fw fa-envelope-square"
                        url: "mailto:your-email"
                        - label: "Website"
                        icon: "fas fa-fw fa-link"
                        url: "your-website"
                        - label: "Twitter"
                        icon: "fab fa-fw fa-twitter-square"
                        url: "your-Twitter"

By default author profile is set `True`. So to assign EKbana Solutions as an author for a post the following YAML Front Matter would be applied:

                author: your-username

If you do not want to show author set `author_profile: false`.

## B: Posts
### 1. Name your post
To name your post follow:

                YEAR-MONTH-DAY-title.md
                
                eg:
                2020-10-06-test-post.md

### 2. Add Front matter to your post 
Sample Front matter:

                ---
                title: "All about EKbana Study Group"
                excerpt: "Something about post"
                excerpt_separator: "<!--more-->"
                last_modified_at: 2020-10-02T16:20:02-05:00
                categories:
                - your-post-category
                tags:
                - your-tag
                - your-tag
                header:
                image: "https://images.pexels.com/photos/772803/pexels-photo-772803.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"
                caption: "Photo credit: [**Pexel**](https://images.pexels.com/)"
                image_description: "A description of the image"
                teaser: "https://images.pexels.com/photos/772803/pexels-photo-772803.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"

                author: your-username
                ---
*Note* Replace sample data with yours.

### excerpt
Write unique excerpt descriptions for each post for improved SEO and archive listings.

                excerpt: "A unique line of text to describe this post that will display in an archive listing and meta description with SEO benefits."

### Adding teaser image
                header:
                        teaser: /assets/images/teaser/my-awesome-post-teaser.jpg

### Adding header image
From local folder

                header:
                        image: /assets/images/image-filename.jpg
                        image_description: "A description of the image"
                        caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
From URL

                header:
                        image: http://some-site.com/assets/images/image.jpg
                        image_description: "A description of the image"
                        caption: "Photo credit: [**Unsplash**](https://unsplash.com)"

To work more with this image and add overlay refer [this link](https://mmistakes.github.io/minimal-mistakes/docs/layouts/#header-overlay)

### Adding Table of Content
Tabel of contents are created by default. Header with ## is at the top-level followed by ### and so on. To disable ToC add following in your front-matter:
                
                toc: false


### 3. Add Images(Standard)
Prepending the filename with {{ site.url }}{{ site.baseurl }}/assets/images/

<img src="{{ site.url }}{{ site.baseurl }}/assets/  images/filename.jpg" alt="">

Or

        ![alt]({{ site.url }}{{ site.baseurl }}/assets/images/filename.jpg)

Image that fills page content container by adding the .full class with:

HTML:

        <img src="{{ site.url }}{{ site.baseurl }}/assets/images/filename.jpg" alt="" class="full">

or Kramdown:

        ![alt]({{ site.url }}{{ site.baseurl }}/assets/images/filename.jpg){: .full}

To add gallery refer [this link](https://mmistakes.github.io/minimal-mistakes/docs/helpers/#gallery)
To add responsive video refer [this link](https://mmistakes.github.io/minimal-mistakes/docs/helpers/#responsive-video-embed)

### 4. Links 

#### Link posts within site

        [name]({% post_url /subfolder/YYYY-MM-DD-file_name_without_md %})
        eg:
        [SASL_Kerberos]({% post_url /Big\ Data/2020-10-02-sasl_kerberos %})


#### Other links with in site
Do not just link the whole URL if it is with in the site.

        Bad approach:
                [Linrear Algebra](https://ekbanaml.github.io/categories/#linear-algebra)

        Good approach:

                [name](({{ site.baseurl }}/<path here>)
                Eg:
                [Linear Algebra](({{ site.baseurl }}/categories/#linear-algebra) 

## C: Assets

1. Place your assets at `/assets/images/category/image-name`