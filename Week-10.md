---
title: Week-10
dateModified: 2024-12-13
dateCreated: 2024-08-20
tags: [react]
parent: "[[Intro to React V3]]"
week: 10
topics: [including graphical elements, styling]
content: lesson plan
---

# Week-10

> [!drafting note] #drafting-note
> use this as an opportunity to add in centralized toast message and clean up error handling

## Introduction

### Topics Covered

- Styling
- Including Graphic Elements

### Lesson Objectives

By the end of this lesson, we will:

#### Objective 1: Styling

- examine several approaches to adding styles to a React application
- author styling using css modules
- author styling using styled-components

#### Objective 2: Including Graphic Elements

- discuss techniques used to include imagery
- design UI elements incorporating vector graphics
- describe how to insert dynamic imagery fetched from an external source

## Discussion Topics

### Styling

> [! note]
> [Sass](https://sass-lang.com/) aka SCSS, is a popular extension CSS but is complex enough that learning it fully will distract from the main lessons. We will not be including it in this course.

- centralized styling in a style sheet works but is not a best practice
	- no scoping - selectors apply across whole applications
		- aka global scope
		- requires discipline to maintain
			- [SMACSS](https://smacss.com/)
			- [BEM](https://en.bem.info/methodology/quick-start/)
		- class naming conventions must account for all portions of app
			- names specific enough identify specific elements but remain short + usable
		- creating new selectors becomes harder as a codebase increases in side
			- harder to read selectors - difficult to figure out where it is meant to apply
			- selectors get larger and more brittle (easier to break during future code changes)
			- troubleshooting undesired style changes becomes common
- CSS Modules
	- CSS file associated with a specific component
		- name is usually `ComponentName.module.css`
	- imported into file (may need to update linting rule)
	- all class (and animation) names are scoped locally
		- prevents styles from applying outside component
		- a few gotchas with sub-components
		- class + element compound selectors good approach to avoid giving all elements class names
	- non-class-name selectors still apply globally
		- class naming preferred in selectors
		- minimize the use of this to top level styles of the app
- Styled-components
	- 3rd party library that styles components
	- based off tagged template literals
		- write styling inside component file
		- produces + attaches style sheet to component
	- prevents class-name problems by scoping (similar to css-modules)
	- does a lot of post-processing and optimization under the hood
		- injects styles only when they are needed
		- applies vendor prefixes
	- makes dynamic styling easily
	- works on html elements and custom components

### Including Graphic Elements

- imagery that is core to the application
	- part of the codebase in
		- `public/` - referenced by src attribute - "public/cat-sleeping.jpg"
			- Not imported into any component files
			- Vite build does not modify file or name
			- Location is configurable - we won't be doing anything with that though
		- `assets/` - referenced through imports in component files
			- Vite creates a url where the asset will be held in the rendered project
			- Vite hashes the filename in deployed code to handle cache invalidation
	- pictures that don't change - hero images, background images
	- small GUI elements - arrows, icons for buttons, logos
- imagery that changes or is dynamically loaded
	- profile or product pictures
	- article photos or illustrations
	- fetched from external source
	- accessed by using src attribute on `<img />`
- easiest to control size by wrapping
	- 100% width on image so it takes up available space
	- apply sizing on wrapping element
- `<img />` events - onLoad, onError
	- animate in or show backup image
- SVG
	- XML-based vector graphic format
	- use in img
	- convert to component - [vite-plugin-svgr - npm](https://www.npmjs.com/package/vite-plugin-svgr)

## Weekly Assignment Instructions

### Expected App Capabilities

#### Stretch Goal: Improve Working with Styled-Components

- make use of babel-plugin-styled-components in the development process

After completing this week's assignment, the app should be able to:

- capability 1
- capability 2
- capability n

### Instructions Part 1

 1. [ ] step 1
 2. [ ] step 2
 3. [ ] step n

### Instructions Part 2

 1. [ ] step 1
 2. [ ] step 2
 3. [ ] step n

### Instructions Part n

 1. [ ] step 1
 2. [ ] step 2
 3. [ ] step n

## References and Further Reading

### Styling

[Features - CSS Modules (Vite docs)](https://vitejs.dev/guide/features#css-modules)
[CSS-Modules docs (GitHub)](https://github.com/css-modules/css-modules)
[styled-components docs](https://styled-components.com/docs)
[vscode-styled-components - VS Code Extension](https://marketplace.visualstudio.com/items?itemName=styled-components.vscode-styled-components)

### Including Graphic Elements

[Features - Static Assets (Vite docs)](https://vitejs.dev/guide/features#static-assets)
[Using SVG (CSS-Tricks)](https://css-tricks.com/using-svg/)
[SVG Properties In CSS Guide (CSS-Tricks)](https://css-tricks.com/svg-properties-and-css/)
[How to Convert SVGs into React Components (Fabio Raminhuk)](https://fabra.dev/blog/converting-svgs-into-react-components-guide)
[Sizing items in CSS (MDN)](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Sizing_items_in_CSS)
[Images, media, and form elements - Sizing images (MDN)](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Images_media_form_elements#sizing_images)
[vite-plugin-svgr - npm](https://www.npmjs.com/package/vite-plugin-svgr)
