If I wanted to make a star rating utility on a web site, which would be more *efficient*?

1. Make a list element with image tags in each list item.
2. Use styling to float image tags within a single container element.

###List Elements

Syntactically this makes the most sense, however there is more markup needed.

###Float Images in Container

Less **DOM** elements, but potential styling support issues with a class like `.star { float: left; }`?