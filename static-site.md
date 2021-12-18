# static sites

1. custom dns
1. https (letsencrypt or github pages)
1. github/github pages to store source files (versioning and better management of files)
1. pugjs templates to make efficient (templates more efficient than html)

* https://www.name.com/api-docs/types/searchresult
* https://www.name.com/api-docs/Domains#CreateDomain
* https://developer.github.com/v3/repos/pages/
* https://stripe.com/docs/payments#build-your-own

## design

a base js library is included in every page which works with github to retrieve and format content, github essentially is the backend store that provides versioning and hosting.  there is a problem with seo since the html will look the same for each site, what gets loaded will vary according to each user, but this requires more exploration since seo is important for marketing sites.

## user/content developer workflow

1.  fork repository, this is not user visible but provides insight into the mechanics of how things work, each repository has an index.html file that includes the js library which is the engine that does all the work.
1.  for the design of the look and feel, one way to do this is to include a sort of description file in the repository.  another way is to have the community create a set of different templates/repositories, you could fork from these.
1.  write articles in markdown, markdown is simple, easy to learn and lightweight, the focus would be on the content more so than look and feel.
1.  commit articles to github which instantly publishes, there should be some pre-check process to make sure things are ok to commit, i guess this can be as close as possible to compilation, i would look to this as more of a preview, not a compile.  for people making websites, having to compile from something to the final html format is too much.  even though a preview would essentially be the same, i guess all the code is hidden to the user, and users should check their articles before commiting.
1.  hosting leverages github pages, shouldn't require godaddy or cloud servers, these are just a bunch of text files.  in the future there could be a play to go downstream and provide hosting, there's nothing cleaner than hosting static pages, you could probably host hundreds of thousands of user's content on a single server, but this won't be the initial focus.
1.  optionally use dns name with integration to name.com or other dns service providers.

## webpage user workflow

1.  user opens dns name, this gets directed to a github pages backend
1.  pages served by github which loads the initial index.html page, this loads the js library which then pulls data from a specific github repository and renders the page within the client browser.


## open issues

open issues that need to be sorted out: 

* how do you host the repositories, does this mean each user has to have their own github account?  this makes the sign up process somewhat annoying, many users will have no clue what github is, probably won't have existing accounts.  or is github hidden from the user?  a single user that has multiple repositories, but this seems like a privilege issue.  ultimately removing github as a dependency makes this cleaner, but github provides a lot of flexibility and tooling like the version/history checking, a nice web frontend.
* could we leverage some sort of blockchain technology that has nothing to do with crypto currency?  the main issue with hosting is that this is not the business i want to get into, i just want to leverage the best technology out there to help accomplish things.
* basically this service becomes a wrapper on top of github pages, will there be any legal issues of doing this?  i think initially if there's little user activity, this probably won't even be a blip on their radar.
* what is the interface for the web designer, should it be a web admin interface or a cli tool?  i kind of like cli tool since this would be a distributed service, but at the end of the day, there might need to be a centralized api.
* statically generated sites also provide the ability to cache content for speed and distribution efficiencies, i think in the end, there still needs to be the process of compiling from template to static pages, but this can be abstracted from the user