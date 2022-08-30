# Part 1: scaffold a component styled NextJS app
Derived from [next-auth-example](https://github.com/nextauthjs/next-auth-example)

## 1. Create Next App
```

git clone https://github.com/nextauthjs/next-auth-example.git next-base-with-auth
cd next-base-with-auth
yarn
```
start a commentary in README.md

create a git repo `next-base-with-auth` and push first commit
```
git init
git add .
git commit -m "first commit"
git branch -M main
git remote rm origin
git remote add origin https://github.com/cg2p/next-base-with-auth.git
git push -u origin main
```

## 2. NextAuth up and running
Copy `.env.local.example` to `.env.local`

Add in auth provider details e.g.
```
GOOGLE_ID=xxxx
GOOGLE_SECRET=xxxx
```

edit out providers not needed in `pages/api/auth/[...nextauth.js]`
```
import NextAuth, { NextAuthOptions } from "next-auth"
import GoogleProvider from "next-auth/providers/google"

// For more information on each option (and a full list of options) go to
// https://next-auth.js.org/configuration/options
export const authOptions: NextAuthOptions = {
  // https://next-auth.js.org/configuration/providers/oauth
  providers: [
    GoogleProvider({
      clientId: process.env.GOOGLE_ID,
      clientSecret: process.env.GOOGLE_SECRET,
    }),
  ],
  theme: {
    colorScheme: "light",
  },
  callbacks: {
    async jwt({ token }) {
      token.userRole = "admin"
      return token
    },
  },
}

export default NextAuth(authOptions)
```
and test authentication through provider
```
yarn dev
```

## 3. Create a Custom Document
Create a Custom Document that will update <html> and <body> on all pages ([docs](https://nextjs.org/docs/advanced-features/custom-document))

`pages/_document.tsx`
```
import { Html, Head, Main, NextScript } from 'next/document'

export default function Document() {
  return (
    <Html>
        <Head>
        <link
            rel="stylesheet"
            href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap"
          />
        </Head>
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  )
}
```

## 4. Streamline configuration
Create `src` and `styles` folders.

Add entries to `tsconfig.json` to make path coding more streamlined
```
//tsconfig.json
{
  "compilerOptions": {

    ...
    "baseUrl": ".",
    "paths": {
      "@/src/*": ["src/*"],
      "@/styles/*": ["styles/*"]
    }
    ...

  }
}
```

Update GitHub repo.

## 5. Switch to Saas styline
Install SaaS
```
yarn add sass@1.51.0
```

Switch to Sass styling. 

In the `styles` folder change `Home.module.css` and `globals.css` to be `.scss` filename.
Update the style import reference in `index.tsx` and `_app.tsx`.

Update `next.config.js` to compile in Sass:
```
/** @type {import('next').NextConfig} */
const path = require('path')

const nextConfig = {
  reactStrictMode: true,
  swcMinify: true,
  sassOptions: {
    includePaths: [path.join(__dirname, 'styles')],
  },
}

module.exports = nextConfig
```

add empty `app.scss` to `styles`. This is where page content styling will be pulled in by `_app.tsx`
update `_app.tsx`
```
import '@/styles/globals.scss';
import '@/styles/app.scss';
```

Reset the global styles, update `global.sccs` with this
```
html {
  box-sizing: border-box;
  font-size: 16px;
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen,
    Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
}
*,
*:before,
*:after {
  box-sizing: inherit;
}
body,
h1,
h2,
h3,
h4,
h5,
h6,
p,
ol,
ul {
  margin: 0;
  padding: 0;
  font-weight: normal;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-weight: bold;
}

ol,
ul {
  list-style: none;
}

img {
  max-width: 100%;
  height: auto;
}

a {
  color: inherit;
  text-decoration: none;
}
```
