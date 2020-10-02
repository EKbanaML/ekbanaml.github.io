# Guidelines to post in EKbanaML study group

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

## Links with in site

        [name]({% post_url YYYY-MM-DD-file_name_without_md %})
        eg:
        [SASL_Kerberos]({% post_url 2020-10-02-sasl_kerberos %})