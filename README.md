# sodapop-search

A lightweight search plugin for Eleventy, built with Lunr and React.

## Description

Sodapop Search was originally built for an [Eleventy](https://www.11ty.dev/) [Refresher](https://github.com/simonhildebrandt/refresher), but has now been extracted as a standalone package - hopefully others will find it useful.

(The examples below are drawn from the [Refresher](https://github.com/simonhildebrandt/refresher) codebase - you can see that source code [here](https://github.com/simonhildebrandt/refresher).)

## Installing the search component

The main component is built with React, and can easily be added into your Eleventy project like this:

```
  eleventyConfig.addPassthroughCopy({
    "./node_modules/@apocryphilia/sodapop-search/lib/sodapop-search.js":
      "./assets/sodapop-search.js",
  });
```

(It's designed to be styled for the site in question - [check the React source](https://github.com/simonhildebrandt/sodapop-search/blob/main/src/index.jsx) for hints on what class names to target - but it also [comes with a default minimal style](https://github.com/simonhildebrandt/sodapop-search/blob/main/src/sodapop-search.css) that might be enough:)

```
  eleventyConfig.addPassthroughCopy({
    "./node_modules/@apocryphilia/sodapop-search/lib/sodapop-search.css":
      "./assets/sodapop-search.css",
  });
```

Next, add a 'search-root' component ([like this](https://github.com/simonhildebrandt/refresher/blob/main/includes/raw.liquid#L32C7-L32C35)) for the React component to mount on to.

Then, just dispatch a 'sodapop-search' custom event ([like this](https://github.com/simonhildebrandt/refresher/blob/main/includes/raw.liquid#L20)) and the component will become visible.

## Installing the search corpus

The second part we need is the search index - we pair [Lunr](https://lunrjs.com/)'s client-side search with it's index builder on the server side, via an [Eleventy Javascript template](https://www.11ty.dev/docs/languages/javascript/#optional-data-method) class.

In a file called `corpus.11ty.mjs` ([here's Refresher's version](https://github.com/simonhildebrandt/refresher/blob/main/refresher/corpus.11ty.mjs)) we can add this:

```
import search from "@apocryphilia/sodapop-search";

export default search;
```

When Eleventy renders your site, the [indexing code](https://github.com/simonhildebrandt/sodapop-search/blob/main/lib/eleventy-index.mjs) will generate a prebaked Lunr index based on the titles, urls and page content of your `all` collection - which the search component can then load and search across.

## Development

So far this machinery is very targeted to [Refresher](https://github.com/simonhildebrandt/refresher)'s usecase, and isn't customisable at all. Any suggestions for ways to adapt it to make it more broadly useful are welcome.
