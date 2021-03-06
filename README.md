# next-google-fonts

> A tiny [`next/head`][next/head] helper for loading Google Fonts **fast** and **asynchronously** ⏩

![NPM Version](https://badgen.net/npm/v/next-google-fonts)
![Types Included](https://badgen.net/npm/types/next-google-fonts)
![Minizipped size](https://badgen.net/bundlephobia/minzip/next-google-fonts)
![License](https://badgen.net/github/license/joe-bell/next-google-fonts)
![NPM Downloads](https://badgen.net/npm/dm/next-google-fonts)

## Table of Contents

1. [Setup](#setup)
2. [FAQs](#faqs)

## Setup

In this example, we're going to add [`Inter`](https://fonts.google.com/specimen/Inter) (specifically weights `400` and `700`) to a Next.js app.

See [joebell.co.uk](https://joebell.co.uk) for a working example.

1. Add the package to your Next.js project:

   ```sh
   npm i next-google-fonts
   ```

2. If you haven't already, create a [custom `Document`](https://nextjs.org/docs/advanced-features/custom-document).

   Place `next-google-fonts` inside `next/document`'s `Head` component, and pass your Google Fonts URL via the `href` attribute:

   ```jsx
   // pages/_document.js
   import Document, { Html, Head, Main, NextScript } from "next/document";
   import GoogleFonts from "next-google-fonts";

   class MyDocument extends Document {
     static async getInitialProps(ctx) {
       const initialProps = await Document.getInitialProps(ctx);
       return { ...initialProps };
     }

     render() {
       return (
         <Html>
           <Head>
             <GoogleFonts href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap" />
           </Head>
           <body>
             <Main />
             <NextScript />
           </body>
         </Html>
       );
     }
   }

   export default MyDocument;
   ```

   Alternatively, if you don't want Google Fonts on every page, you could roll your own [custom Head component](#how-do-i-use-next-google-fonts-in-a-custom-head-component).

3. Add the requested Google Font/s to your styles with a sensible fallback.
   It really doesn't matter whether you're using CSS or Sass or CSS-in-JS:

   ```css
   body {
     font-family: "Inter", sans-serif;
   }
   ```

4. [Run your Next.js app](https://nextjs.org/docs/api-reference/cli#build) to see the results in action!

   You should expect to see the fallback font first, followed by a switch to the Google Font/s without any render-blocking CSS warnings. Your font/s will continue to display until your app is re-hydrated.

   If JS is disabled, only the fallback font will display.

## FAQs

- [Why?](#why)
- [How do I use `next-google-fonts` in a custom `Head` component?](#how-do-i-use-next-google-fonts-in-a-custom-head-component)

### Why?

`next-google-fonts` aims to make the process of using Google Fonts in Next.js more consistent, faster and painless: it preconnects to font assets, preloads and asynchronously loads the CSS file.

In the current iteration of [`next/head`][next/head], we can't make use of the familiar "media hack" method of asynchronous Google font loading:

```html
<!-- Plain HTML -->
<link
  rel="stylesheet"
  href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap"
  media="print"
  onload="this.media='all'"
/>
```

If you'd like to track this issue in Next.js, you can follow it here: [Next.js#12984](https://github.com/zeit/next.js/issues/12984).

### How do I use `next-google-fonts` in a custom `Head` component?

It's important to acknowledge that `next-google-fonts` is a small [`next/head`][next/head] component and should **not** be nested inside [`next/head`][next/head]. To solve this, wrap both components with a `Fragment`.

```jsx
// components/head.js
import * as React from "react";
import NextHead from "next/head";
import GoogleFonts from "next-google-fonts";

export const Head = ({ children, title }) => (
  <React.Fragment>
    <GoogleFonts href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap" />
    <NextHead>
      <meta charSet="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <meta httpEquiv="x-ua-compatible" content="ie=edge" />

      <title>{title}</title>

      {children}
    </NextHead>
  </React.Fragment>
);
```

[next/head]: https://nextjs.org/docs/api-reference/next/head
