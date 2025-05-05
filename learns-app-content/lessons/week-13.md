## Discussion Topics

### Snapshot of the React Ecosystem

#### Frameworks that Use React

Through the course, we have used Vite + React to practice what we have learned by building out projects. Vite provides a very friendly start to your React journey and can even be used successfully in small to medium projects. It is a solid foundation! For some products, especially those of larger size, it can be beneficial to adopt a framework that use React instead of just using React and a development server. These frameworks have solved some of the most common challenges in developing full-scale applications and often provide powerful routing and state management options. Some also make use of React Server Components or their own systems to form full-stack applications. All of the frameworks listed below are recommended by the React team as options for larger production applications.

**[Next.js](https://nextjs.org/docs)**: Summary of features: file-based routing, client and server side component rendering, improved data fetching, optimization of assets and scripting to improve app performance.
**[React Router](https://reactrouter.com/start/framework/installation)**: Summary of features: file-based or declarative routing, [type safety](https://en.wikipedia.org/wiki/Type_safety), multiple rendering strategies
**[TanStack Start](https://tanstack.com/start/latest)**: Summary of features: client and server-side rendering, type safety, built-in fetch, automatic caching and preloading, async state management.
**[RedwoodJS](https://redwoodjs.com/)**: Summary of features: deployment flexibility, CLI tools, declarative routing, declarative data fetching, GraphQL, Prisma as a backend.

#### Component and UI Libraries

Similar to frameworks, these libraries solve common challenges that React developers face. These tend to focus on solving complex UI requirements or improving the development experience. Some provide well-polished components that assist with accessibility or creating intuitive and pleasant interfaces. Others focus on theming or providing starting points for design systems.

**[HeroUI (Previously NextUI)](https://www.heroui.com/docs/guide/introduction)**: Combines Tailwind and React Aria and provides a customizable design system for attractive interfaces.
**[Tailwind CSS](https://v2.tailwindcss.com/docs)**: Utility class-based CSS framework.
**[MUI](https://mui.com/)**: Google's Material Design implemented in React components.
**[Headless UI](https://headlessui.com/)**: Un-styled components that focus on solving accessibility challenges.
**[Radix UI](https://www.radix-ui.com/)**: Pre-styled component library that focuses on providing common design elements such as dialogs, menus, forms, cards, and many others.
**[React Bootstrap](https://react-bootstrap.netlify.app/)**: Bootstrap adapted to React applications - provides styling and common design elements
**[Chakra UI](https://chakra-ui.com/)**: Themeable components for common design elements. Uses props for styling.

#### Other React Libraries

##### State Management

[Zustand](https://zustand.docs.pmnd.rs/getting-started/introduction)- State management library that is lighter than redux but solves some complex state management pitfalls.
**[Redux Toolkit](https://redux-toolkit.js.org/introduction/getting-started)**- Centralized state management. It's built on top of [Redux](https://redux.js.org/) with the aim to reduce the amount of boilerplate code needed to perform state management tasks.

##### Animation

[Motion](https://github.com/motiondivision/motion)
[react-spring](https://www.react-spring.dev/)
[GSAP](https://gsap.com/)

##### Data Fetching

**[TanStack Query](https://tanstack.com/query/latest/docs/framework/react/overview)**: Assists with fetching, caching, and synchronizing server state. It comes with TanStart but can also work independently of that framework.
**[Axios](https://axios-http.com/docs/intro)**: Build on the older [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) but still used because of its extensive API that makes certain data fetching tasks easier.

##### Misc

**[react-toastify](https://www.npmjs.com/package/react-toastify)**: In-browser, customizable user notifications
**[Formik](https://formik.org/)**: Improves form development by handling state values, validation, errors, and form submission.,
**[uniforms](https://uniforms.tools/)** - Set of libraries that generate forms from developer-defined schemas.

#### VS Code Plugins

Editing code in a plain text editor is a valid approach that is used successfully by some developers but a lot of us like to extend our code editor's capabilities. The list below provides some recommendations for those who are interested in enhancing their productivity and experience with VS Code. These extensions range from visual tweaks to tools that can assist in project management.

Enhancing your coding experience goes beyond basic text editing and can greatly improve productivity. While some developers favor a straightforward approach with plain text editors, extending VS Code can elevate your coding workflows to new heights. This curated list of VS Code extensions offers a range of tools and features that enhance productivity, streamline project management, and customize the coding environment to suit your individual preferences.

**[JS JSX Snippets](https://marketplace.visualstudio.com/items?itemName=skyran.js-jsx-snippets)**: Expands preconfigured keywords into commonly typed code snippets.
**[CSS Var Complete](https://marketplace.visualstudio.com/items?itemName=phoenisx.cssvar)**: Intellisense support for CSS custom properties.
**[GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)**: AI agent by GitHub that helps with coding tasks.
**[Highlight Matching Tag](https://marketplace.visualstudio.com/items?itemName=vincaslt.highlight-matching-tag)**: Easily identify a matching tag when one is selected.
**[Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)**: Improves markdown functionality
**[markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)**: Linter that highlights parts of a markdown file that violates rules based in best practices.
**[Todo Tree](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree)**: Searches a repo for all todos and comment keywords and surfaces them in a directory-like structure. This can also be configured to highlight console statements so you don't forget to remove them.

#### Other Development Tools

The IDE, terminal, and documentation are the most important tools in a React developer's arsenal but there are other tools to consider.

##### Working with APIs

These tools help developers work with APIs by saving all aspects of fetch requests: endpoints, methods, headers, payloads, etc. Both employ environmental variable that can be interpolated into the requests so they be re-used between locally hosted, sandbox, and live backend applications. Postman has many other features including pre and post fetch scripting but tends to run slowly at times. HTTPie is the lighter of the two listed.

**[Postman](https://www.postman.com/)**:
**[HTTPie](https://httpie.io/desktop)**:

##### Developer's Notebook

Keeping a development journal is important for developers for several reasons:

- **Problem Solving**: Writing down issues, errors, and solutions can help in problem-solving. Keeping notes organized in a single system can serve as a reference to revisit when facing similar problems in the future.
- **Knowledge Sharing**: You can use a journal to capture insights, tips, and best practices and share your learning with others in the community. It contributes to collective knowledge and fosters collaboration.
- **Learning and Growth**: Reflecting on past projects, experiments, and experiences can aid in learning from mistakes and successes. It promotes continuous learning and personal growth.
- **Enhanced Focus and Organization**: Maintaining a journal can help you stay organized, set goals, prioritize tasks, and maintain focus on projects.

**[Joplin](https://joplinapp.org/)**: Open source note taking application that organizes markdown notes into notebooks.
**[Obsidian](https://obsidian.md/)**: Free, markdown based digital notebook with a range of productivity and styling plugins.

##### Databases

Although we did not work with databases during this course, they are a common part of any web application. Using a database manager helps with development tasks such as examining relational structures, populate or remove test data.

**[DBeaver Community | Free Universal Database Tool](https://dbeaver.io/)**:

##### TypeScript

**[TypeScript](https://www.typescriptlang.org/)** is a static typing syntax placed on top of JavaScript that streamlines the development process and can enhance the overall quality of a codebase. Static types (types that can't be accidentally changed) help developers better understand the data that they are working with. It provides information about what type of data to pass into functions and other interfaces and the values they return.

VS Code and other modern web development IDEs give developers live feedback and autocompletion geared towards writing cleaner and more reliable code. By defining types for variables, props, and state, and using types found in most libraries, developers can catch errors which results in fewer runtime issues. It also makes the codebase more accessible to new developers joining the project and simplifies the onboarding process.
