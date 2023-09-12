# GuruTechSummitReact

## Tools we will be using
- [Node.js](https://nodejs.org/en/)
- [NPM](https://www.npmjs.com/)
- [Nx](https://nx.dev/)
- [React](https://reactjs.org/)
- [Tailwindcss](https://tailwindcss.com/)
- [PostCSS](https://postcss.org/)
- [Autoprefixer](https://autoprefixer.github.io/)
- [Playwright](https://playwright.dev/)
- [Webpack](https://webpack.js.org/)
- [Babel](https://babeljs.io/)
- [ESLint](https://eslint.org/)
- [Jest](https://jestjs.io/)
- [TypeScript](https://www.typescriptlang.org/)
- [Prettier](https://prettier.io/)
- [Fontello](https://fontello.com/)

## How to generate this app

```bash
npx create-nx-workspace@latest guru-tech-summit-react
```

```bash
? Workspace name (e.g., org name)             guru-tech-summit-react
? What to create in the new workspace         React
? What framework should we use?               None
? Integrated Monorepo or Standalone           Integrated Monorepo
? Application name                            client
? Bundler                                     Webpack
? E2e Test Runner                             Playwright
? Default stylesheet format                   CSS (I will be using Tailwindcss, so very little use of stylesheets)
? Use Nx Cloud?                               No (I have not had much luck with Nx Cloud but I intend to give it another go sometime)
```

## Test the app

```bash
nx serve client
```

## How to add Tailwindcss

We will use the [Using PostCSS](https://tailwindcss.com/docs/installation/using-postcss) instructions as a reference but we will modify them to our use case.

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init
```

Create a `postcss.config.js` file in the root of the project with the following content:

```js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  }
}
```

Change the generated `tailwind.config.js` file in the root of the project with the following content:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./apps/client/src/**/*.{html,js,ts,jsx,tsx}"],  // This must match the path to your client app.  If you chose to call your app something else, it needs to be reflected here.
  theme: {
    extend: {},
  },
  plugins: [],
}
```

In the `apps/client/src/styles.css` file, add the following content:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

To test that everything is working, modify the `apps/client/src/app/app.tsx` file to look like the following snippet and restart the app:

```tsx
export function App() {
  return (
    <div>
      <h1 className="text-3xl font-bold underline">
        Hello world!
      </h1>
    </div>
  );
}

export default App;
```

Notice the class names added in the above snippet `"text-3xl font-bold underline"`.  These are Tailwindcss class names.  If you see the text in the browser with the correct styling, then you have successfully added Tailwindcss to your app.

## How to create a component library

Nx is flexible and can handle multiple projects in a single workspace.  We will create a library to hold our common components that can be used seamlessly across multiple apps.

```bash
nx g @nx/react:lib common-components
```
```bash
? What unit test runner should be used              Jest
? Which bundler would you like to use               None
```

Make sure to add the path of your library to the `tailwind.config.js` file in the root of the project.  It should look something like this now:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./apps/client/src/**/*.{html,js,ts,jsx,tsx}", "./common-components/src/**/*.{html,js,ts,jsx,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

## How to add a utilities and types library

```bash
nx g @nx/js:library data
```
```bash
? What unit test runner should be used              Jest
? Which bundler would you like to use               None
```

## How to add a shared assets library
Create a new folder in the root directory called `shared-assets`.  This will be a library that will hold all of our shared assets such as fonts, images, etc.

## How to add Fontello
Go to [Fontello](https://fontello.com/).  Add a font name in the font name input at the top of the page (I'll be calling mine `guru-tech-summit-react`).  

I like to change the prefix of my icons to match my project otherwise the prefix defaults to `icon-`. To do this, click on the wrench icon button at the top of the page to open the configuration dropdown.  Change the prefix to whatever you want and click the `Save session` button.  I am naming mine `gtsr-`

You can add icons from the Fontello library or you can upload your own icons.  Once you have added all of the icons you want, you can download the config file to the root of your project and rename the file to `fontello.config.json`.

Add the following lines to the scripts section of the `package.json` file in the root of the project:

```json
"icons:generate": "npx fontello-cli install --config fontello.config.json --css ./shared-assets/fonts/css --font ./shared-assets/fonts/font",
"icons:open": "npx fontello-cli open --config fontello.config.json"
```

Run the following command to generate the fonts and css files:

```bash
npm run icons:generate
```

This will create a `fonts` folder in the `shared-assets` library with the icons you chose on the fontello.com website.

To use the icons in your app, you will need to add the following lines to the `apps/client/src/styles.css` file:

```css
@import '../../../shared-assets/fonts/css/animation.css';
@import '../../../shared-assets/fonts/css/<name-of-your-font>.css';
```

Make sure to change `<name-of-your-font>` to the name of the font you chose on the fontello.com website.

You will also need to register those files with your app in the `apps/client/project.json` file.  Add the following lines to the beginning of the `styles` array:

```json
"shared-assets/fonts/css/animation.css",
"shared-assets/fonts/css/<name-of-your-font>.css",
```

It should look something like this:

```json
"styles": [
  "shared-assets/fonts/css/animation.css",
  "shared-assets/fonts/css/guru-tech-summit-react.css",
  "src/styles.css"
],
```

You can now use the icons in your app.  For example, to use the `gtsr-glass` icon, you would use the following snippet:

```tsx
<i className="gtsr-glass" /> // This is an example of using the icon as a font
```

Make sure to use your prefix in front of the icon name.




# NX-provided README.md

<a alt="Nx logo" href="https://nx.dev" target="_blank" rel="noreferrer"><img src="https://raw.githubusercontent.com/nrwl/nx/master/images/nx-logo.png" width="45"></a>

✨ **This workspace has been generated by [Nx, a Smart, fast and extensible build system.](https://nx.dev)** ✨


## Start the app

To start the development server run `nx serve client`. Open your browser and navigate to http://localhost:4200/. Happy coding!


## Generate code

If you happen to use Nx plugins, you can leverage code generators that might come with it.

Run `nx list` to get a list of available plugins and whether they have generators. Then run `nx list <plugin-name>` to see what generators are available.

Learn more about [Nx generators on the docs](https://nx.dev/plugin-features/use-code-generators).

## Running tasks

To execute tasks with Nx use the following syntax:

```
nx <target> <project> <...options>
```

You can also run multiple targets:

```
nx run-many -t <target1> <target2>
```

..or add `-p` to filter specific projects

```
nx run-many -t <target1> <target2> -p <proj1> <proj2>
```

Targets can be defined in the `package.json` or `projects.json`. Learn more [in the docs](https://nx.dev/core-features/run-tasks).

## Want better Editor Integration?

Have a look at the [Nx Console extensions](https://nx.dev/nx-console). It provides autocomplete support, a UI for exploring and running tasks & generators, and more! Available for VSCode, IntelliJ and comes with a LSP for Vim users.

## Ready to deploy?

Just run `nx build demoapp` to build the application. The build artifacts will be stored in the `dist/` directory, ready to be deployed.

## Set up CI!

Nx comes with local caching already built-in (check your `nx.json`). On CI you might want to go a step further.

- [Set up remote caching](https://nx.dev/core-features/share-your-cache)
- [Set up task distribution across multiple machines](https://nx.dev/core-features/distribute-task-execution)
- [Learn more how to setup CI](https://nx.dev/recipes/ci)

## Connect with us!

- [Join the community](https://nx.dev/community)
- [Subscribe to the Nx Youtube Channel](https://www.youtube.com/@nxdevtools)
- [Follow us on Twitter](https://twitter.com/nxdevtools)
