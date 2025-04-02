## Discussion Topics

### Intro to React

React is a frontend library used by developers to build dynamic user interfaces. React takes a declarative approach to DOM manipulation with the help of React DOM to provide interactivity in web applications, also known as single-page applications or SPA for short. React takes care of:

- assembling components to render a UI
- listening for user events - mouse cursor hovering, typing in a field, button clicks, etc
- updating state[^state] based on events and other inputs
- automatically updating the UI when state changes

That's helpful but what problems does it solve?!

Developing an SPA without libraries (or even with jQuery) is a complicated process for anything but the smallest app. Prior to their rise of libraries like React or frameworks[^libraries-and-frameworks] like Angular and Vue, developers had to take an imperative approach to programming interactivity into an html document. Stated another way, developers have to programmatically describe the processes their SPA needs to use to keep the UI updated.

- Elements must be created, destroyed or managed.
- Event listeners have to be added, configured, and managed.
- Elements then need to be added, removed, replaced, or modified to update the UI.

Often, this includes adding or removing sub-elements that aren't known about ahead of time like list items or images loaded from a remote data source. Each element may need event listeners which in turn, are configured with logic to update the interface. Listeners also need to be managed carefully to keep the application performing smoothly. They don't automatically disappear when elements they are used on are removed or are no longer needed. Forgotten event listeners take up memory on a user's system and can cause serious performance issues or can even crash a browser. This is a lot to manage!

The most of the current front end libraries or frameworks use a declarative approach to programing a UI. Declarative programming allows us to describe the SPA's structure and state. It is then library's/framework's responsibility to accomplish the all the tasks needed to keep the UI updated as state changes. As a consequence, it allows us to create complex SPAs with relative ease compared to approaches that do not use a framework.

React's strength comes from its use of components and the way it keeps the UI updated. Components allow us to divide an SPA into smaller modules consisting of page elements, styling, and state logic. In programming terms, components encourage "separation of concerns". Components allow us to focus on small portions of the application at a time. Each component does one or a few things rather than having a large files (HTML, CSS, JS) that group all a page's functionality together.

React provides hooks which are functions that allow us to manage state and logic inside of components. Props (properties) are used to pass data and event information between components. We will cover hooks and props in future lessons so don't worry if they sound confusing right now.

Another key aspect of React is its use of a [Virtual DOM](https://legacy.reactjs.org/docs/faq-internals.html#what-is-the-virtual-dom). This is a lightweight copy of the actual DOM (Document Object Model) in memory. When there are changes in the state of a component, React first updates the Virtual DOM and then compares it to the real DOM to identify the changes needed. This [diffing](https://www.geeksforgeeks.org/what-is-diffing-algorithm/) process is more efficient than directly manipulating the entire DOM resulting in fast updates and improved performance.

### App Installation

> [!remember]
> As mentioned during lesson 00, we will be working on two projects through this course. During the lessons, all coding examples are from CTD Swag, an eCommerce store. This is an optional code-along project that will not be turned in. The repo for [CTD Swag can be found here](https://github.com/Code-the-Dream-School/ctd-swag)

To work with a React project, we must choose a build tool and server. One of the easiest ways to get started is using [Vite](https://vitejs.dev/guide/why.html) with a [React template](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-react). Vite is a modern build tool for frontend development that focuses on speed and simplicity. It is designed to provide a fast development experience and has a strong plugin ecosystem that has made it a popular tool in the JavaScript community. We'll look closer into Vite later after installing our app.

Create a new repo in GitHub and give it a name. All of the other options can remain the default. Any files such as the license, readme, or .gitignore will just get in the way of the installation process and be re-created anyways. The .git directory (a normally invisible directory used to manage version control) is unaffected so does not have to be worried about.

![github create new repository](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-01/new-repo.png)

Clone the repo locally. With a terminal opened to the local repo's directory, issue the command below to scaffold out a Vite project using the React template.

```bash
 npm create vite@latest ./ -- --template react
```

Let's break that command down to see what it's doing:

1. `npm create …`: we're using npm's create command. `create` is an [alias](<https://en.wikipedia.org/wiki/Aliasing_(computing)>) of the `init` command which creates our package.json file.
2. `… vite@latest …`: this argument tells npm to scaffold the project using Vite's newest package.
3. `… ./ …` lets npm know to use the current directory.
4. `… -- …` forwards the remaining options to the package used to scaffold the project.
5. `… --template react` tells vite's package to scaffold the project using its React template.

> [!info]
> If we run the command without any options (`npm create vite@latest`) this will start an interactive prompt in the terminal to help scaffold the Vite project. It will ask for a project name, a template to use, and then finally a template variant. Be careful to choose `JavaScript`, not `JavaScript SWC`. [SWC](https://swc.rs/) is a faster bundler that can be used instead of the default one used by Vite but doesn't have the right options for our project. We will not be using TypeScript in this course so don't choose that either.

The scaffolded project includes a starter SPA and a few supporting files. None of the dependencies are installed until we run another command, `npm install`. This installs all the packages listed in the package.json dependencies and can take up to a few minutes to complete. Once it done, we can take a look at the project structure.

```terminal
.
├── .git/...
├── .gitignore
├── README.md
├── ESLint.config.js
├── index.html
├── package.json
├── node_modules/...
├── public
│   └── vite.svg
├── src
│   ├── App.css
│   ├── App.jsx
│   ├── assets
│   │   └── react.svg
│   ├── index.css
│   └── main.jsx
└── vite.config.js

```

1. **.git/**: is an invisible directory created by git to maintain version control. You may not see it if your operating system hides directories and files that start with a ".". VS Code also will hide this directory by default.
2. **.gitignore**: this file lists all those files and directories that should _not_ be tracked with version control.
3. **README.md**: this file contains pertinent information about the project. We keep this up to date with details such as a project description and steps that others need to take to run or work with the project.
4. **ESLint.config.js**: is used to configure [ESLint](https://ESLint.org/), a tool used to identify syntax problems or common [anti-patterns](https://en.wikipedia.org/wiki/Anti-pattern).
5. **index.html**: this file is the [entry point](https://vitejs.dev/guide/#index-html-and-project-root) for the application.
6. **package.json**: this file contains details about the project, some scripts aliases, and a list of all the packages that the project is dependent upon.
7. **public**: this is a [directory](https://vitejs.dev/guide/assets.html#the-public-directory) is used to hold static assets like images and fonts that we want to remain unchanged.
8. **src**: this directory contains the majority of the working files for the application.
9. **vite.config.js**: this file sets [configuration options for Vite](https://vitejs.dev/config/)

To work with the project, we have to start Vite's server. To find the right command(s) look in "scripts" object in package.json.

```json
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "lint": "ESLint .",
  "preview": "vite preview"
}
```

Since the packages in the repo are installed locally, the command line will not recognize them. Instead of trying to call them directly (e.g. typing `vite build` into our terminal), we use npm to call the scripts for us by using the command `npm run <<scriptKey>>` in the terminal at the project's root directory. Remember in JSON, a `key` is a property name - the word on the left side of a colon. In our case, we're going to use the command **`npm run dev`**. This will spin up a development pipeline to create a version of our code that is understandable by the browser.

![vite running in terminal](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-01/terminal-serve.png)

Vite then serves up the transformed code so we can see it in the browser!

![starting page in browser](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-01/starter-page.png)

You may have noticed a `:5173` in the url. This is the port number that Vite serves content from locally. We will talk more about this and how to deploy a live app in week 13.

Any time we are working in our codebase, it's _highly recommended_ to have the development server running and our SPA open in a browser window. This gives instant feedback on the code that we are working on. There are plenty of scenarios where our code does not have any errors in a grammatical or technical sense but will crash our SPA or generate other undesirable behaviors.

### Project Walk-Through

Now that we have the project scaffolded and we know it runs, we'll dig into some of the details about working with our SPA. Vite incorporates several other tools to be aware of and provides us with features to make UI development a pretty nice experience.

#### Sub-tools

- **[esbuild](https://esbuild.github.io/)** - Vite uses esbuild for pre-bundling during development. It converts all of our code and dependencies into native ESM understood by browsers. It also combines project dependencies into a single cached module to improve page loading/refreshing while we code. In other words, rather than having to re-bundle everything every time we save a file while Vite is running, it bundles all of our dependencies and saves the output. It then only has to rebuild the module(s) containing our code.
- **[Rollup](https://rollupjs.org/introduction/)** - this is another module bundler for JavaScript. Vite uses this to output highly optimized files for production.
- **[PostCSS](https://github.com/postcss/postcss)** - PostCSS is a JS tool that transforms CSS through an ecosystem of plugins. We will not be working with this directly.
- **[CSS Modules](https://github.com/css-modules/css-modules)** - this tool scopes class selectors in module files to the respective component file. This simplifies style management directly by preventing selectors from inadvertently applying styles to undesired areas of the rendered page. We'll talk more about this in [[Week-10|week 10]].

#### Features

- **HMR** - Hot Module Replacement. This is the ability to replace a JavaScript module in the browser while maintaining application state. This mean that the app doesn't need to re-start. This is very handy when working on features that are "far away" from the initial state of the application. For example, this could include maintaining a logged on user session where some content is blocked from a user who is not logged in. It gets to be a pain when you have to log into your app 30 times in a row while you're doing work on a new feature. With HMR, this hassle is reduced.
- **TypeScript support** - TypeScript (TS) provides valuable guard rails for developers so that they can develop bug-free, performant JavaScript. Since browsers and Node don't natively support TS, the code has to be [transpiled](https://daily.dev/blog/typescript-transpiler-explained) to JavaScript. While there are tools to do this, they need careful configuration to get them working properly. Vite provides support for this without the need to configure anything.
- **JSX transformation** - Similar to TypeScript, browsers do not understand JSX. This extension of JavaScript (which we will talk more about next week) needs converted to plain JavaScript before being served to a browser. Vite provides this transformation for any `.jsx` or `.tsx` (the TypeScript equivalent) file in the `src/` directory automatically.
- **CSS, JSON importing** - JavaScript files are not normally able to import files that are written in other languages. Rather than having to create special loaders ourselves so that we can work with non-JavaScript files, Vite gives us the ability to do so. Vite injects CSS onto the page and gives it HMR support. Vite also allows us to work with JSON through named or default imports that we can treat it like a JavaScript object. This is handy for data population where we don't want to reach for an API connection.
- **Inclusion of static assets** - these resolve a public URL for the file when imported into a `.jsx` file. We'll explore how to take advantage of this during [[Week-10|week 10]].
- **strong Plugin ecosystem** - Vite has [official plugins](https://vitejs.dev/plugins/) and [community plugins](https://github.com/vitejs/awesome-vite#plugins) that extend its capabilities - some are React-specific, some work with other frameworks, and a lot are UI framework/library agnostic.

### Stretch Goal: Improving the Development Environment

#### Enhance VS Code's Built-in Linting with ESLint

A linter is a tool that performs a [static analysis](https://en.wikipedia.org/wiki/Static_program_analysis)of a codebase to flag syntax errors and bad practices without having to run the code. VS Code already provides basic static analysis for JavaScript but we can extend this with ESLint. The React template includes some sensible default rules when we installed it but we also have to install VS Code's [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) extension. The extension will allow VS Code to lint all project files and adds a tooltip when mousing over a flagged item. The tooltips usually include a brief summary of the rule violation and a link to more documentation. In the screenshot below, clicking the blue text "[react/jsx-key](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-key.md)" will open a browser window to documentation that explains the error, how to resolve it, and even rule configuration options for `eslint.config.js`.

![missing key props error in ide](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-01/missing-key.png)

A keen observer may have noticed a few unusual comments in an earlier screenshot. At the top of the file for the `App` component, there are commented lines that disable two rules. Those lines are suppressing errors ESLint would have flagged.

![eslint rules disabled in file by comment](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-01/eslint-disable.png)
In the following screenshot of an error tooltip, we have an error on `setTestList` being reported by VS Code's built-in static analysis and ESLint. `ts` stands for TypeScript even though it's evaluating JavaScript. This may be confusing since we're not using TypeScript but is just an odd detail resulting from how the internals of VS Code work.

![no unused vars tooltip](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-01/unused-vars.png)

When working with ESLint, we can to add and remove rules that suit our needs. ESLint provides three ways to modify rules:

1. ignore a rule for a file
2. ignore a rule for a single line
3. apply or ignore rules across a codebase

We have already seen how to turn off the rule for the entire file. Another way is to add the appropriate comment directly above a line of code which will then ignore only that instance of the warning. Finally, to change a rule's behavior across the codebase, it can be configured in `eslint.config.js` which is found at the root of the project. We can add, modify, or disable rules with this file. Keep in mind that the rules already in place conform to community best practices. Don't change them just to get rid of the squiggly lines in the code! Make sure you understand the rules you disable or enable and have a strong reason for doing so.

![eslint config rules disabled](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-01/eslint-rules-config.png)

#### Automatic Code Formatting with Prettier

Clean code is vital for any project, especially in a team codebase, but having to think about formatting details detracts from our attention which is better spent on crafting useful code. We can delegate formatting to tool that automates the process of applying formatting rules consistently across a codebase.

One of the most popular JavaScript formatting tools is [Prettier](https://prettier.io/). Like ESLint, we include a configuration file in our codebase and we have to install [a plugin for VS Code](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode). When configured correctly, it frees us from having to think about formatting! Let's go ahead and set that up.

Since this is a development tool and is not a part of the app, we install Prettier as a dev dependency. `npm install --save-dev prettier`. The shorthand of this command is `npm i -D prettier`. After installing, we create a configuration file, `.prettierrc` at the root directory of the project. Inside `.prettierrc` we add a JSON object containing some starting rules:

```JSON
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5"
}
```

We then have to install Prettier plugin and configure VS Code to use it as the default code formatter. After installing the plugin, open VS Code's settings and filter for "format". For `Editor: Default Formatter` select Prettier and then check the box for `Editor: Format on Save`.

![searching for format in ide settings](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-01/search-format.png)

Code before and after saving with Prettier enabled:

![code before and after prettier formatting](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-01/pretter-before-after.png)

- semicolons are consistently used
- quotes on imports are single quotes instead of double quotes
- the spacing between the array's brackets and its elements has been removed
- there are only one empty line between code blocks

### Summary

We were introduced to React its benefits for front end web developers. We also set a repo, installed a development server, and got a React project spun up. We covered some basics about working with Vite and introduced optional tools that will make our development experience much nicer. We have covered a lot of material this first week! Armed with this knowledge, it's time for you to set up the project that you'll be submitting weekly.

In contrast, traditional multi-page websites follow a server-centric model where navigating between pages involves full-page reloads, resulting in slower interactions and delays in content delivery. Each page request triggers a server response to load a new page, leading to a less interactive and dynamic user experience compared to SPAs. Multi-page websites have distinct HTML files for each page and rely on server-side rendering to generate and serve content, which can be less efficient and scalable for complex web applications.

SPAs revolutionized web development by optimizing performance and user experience through client-side rendering and dynamic content updates. By eliminating the need for full-page reloads and reducing server requests, SPAs deliver faster load times and smoother interactions. While SPAs excel in creating interactive and responsive applications, multi-page websites remain relevant for content-heavy platforms that benefit from SEO advantages, as search engines find it easier to crawl individual pages. The choice between an SPA and a traditional multi-page website depends on the specific requirements of the application, balancing factors like user experience, performance, and search engine visibility.

[^state]: State refers to the current condition or data within an application at a specific point in time. It includes all data relevant to the application, such as user input, server responses, and UI state.

[^libraries-and-frameworks]: Libraries and frameworks are code packages written to solve complex or specialized challenges. We incorporate libraries or frameworks into our projects to simplify the development process. The distinction between the libraries and frameworks is a nuanced topic and can be interpreted differently based on their programming language and what they're used for. "[Inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control)" is a common element to most comparisons between the two.

With a library, we are in control of how its code is called. Libraries provide a toolset that we use by calling application programming interfaces (APIs) that they provide. Conversely, a framework defines how the application works then calls our code to configure its behavior and to determine what to include. They tend to be larger in scope and provide more programming tools for us to use.

**Example UI libraries**: [jQuery](https://jquery.com/), [Knockout](https://knockoutjs.com/), [Preact](https://preactjs.com/), and [React](https://react.dev/).
**Example UI frameworks**: [Angular](https://angular.dev/), [Astro](https://astro.build/), [Ember](https://emberjs.com/), [Next.js](https://nextjs.org/), [Remix](https://remix.run/), and [Vue](https://vuejs.org/).
