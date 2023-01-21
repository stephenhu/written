# blog

write content in pure markdown for its ease of syntax.  style the page with some extension of markdown
entirely in js.  index page gets raw markdown files from cdn or some externally visible location, renders
pages on the fly to show client.  an example is that all md files stored in a giithub public repo, which is stored coincidentally on s3, this gets read in during the initial load of the page and rendered in js to show on the client browser.  basically the index page provides styling for the markdown content, but how do you tell the page which md files to read?  perhaps there's an index somewhere which then has a list of all the pages.