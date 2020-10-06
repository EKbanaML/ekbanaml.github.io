# Guidelines to post in EKbanaML study group


## Naming your post
To name your post follow:

                YEAR-MONTH-DAY-title-category.md
                
                eg:
                2020-10-06-test-post-cpp-class.markdown
`category ` is optional but recommended.

## Adding User
Add details to: `/_data/authors.yml`

                ekbana:
                        name    : "EKbana Solutions"
                        bio     : ""
                        avatar  : "/assets/images/userPhoto/no_photo.jpg"
                        links:
                        - label: "Email"
                        icon: "fas fa-fw fa-envelope-square"
                        url: "mailto:<email>"
                        - label: "Website"
                        icon: "fas fa-fw fa-link"
                        url: "<website>"
                        - label: "Twitter"
                        icon: "fab fa-fw fa-twitter-square"
                        url: "<Twitter>"

By default author profile is set `True`. So to assign EKbana Solutions as an author for a post the following YAML Front Matter would be applied:

                author: ekbana

If you do not want to show author set `author_profile: false`.

## excerpt
Write unique excerpt descriptions for each post for improved SEO and archive listings.

                excerpt: "A unique line of text to describe this post that will display in an archive listing and meta description with SEO benefits."

## Adding teaser image
                header:
                        teaser: /assets/images/teaser/my-awesome-post-teaser.jpg

## Adding header image
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

## Adding Table of Content
                toc: true
                toc_label: "My Table of Contents"
                toc_icon: "cog"


## Sample Front matter:

                ---
                title: "All about EKbana Study Group"
                excerpt_separator: "<!--more-->"
                last_modified_at: 2020-10-02T16:20:02-05:00
                categories:
                - EKbana Study Group
                tags:
                - ekbana
                - ml
                header:
                image: "https://images.pexels.com/photos/772803/pexels-photo-772803.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"
                caption: "Photo credit: [**Pexel**](https://images.pexels.com/)"
                image_description: "A description of the image"
                teaser: "https://images.pexels.com/photos/772803/pexels-photo-772803.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"

                author: ekbana
                ---


## Images(Standard)
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

## Links with in site

        [name]({% post_url YYYY-MM-DD-file_name_without_md %})
        eg:
        [SASL_Kerberos]({% post_url 2020-10-02-sasl_kerberos %})



