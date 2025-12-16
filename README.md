# Onboarding Tutorial

This tutorial will guide you through your first steps as a front-end programmer.

### Prerequisites

Take a quick look at the Vue TypeScript Coding Style Guide (hereon referred to as the Coding Style Guide), sections 1 to 3. It introduces the architecture we use.

Here are the programs and packages that you should install:

* [**VS Code**](https://code.visualstudio.com/Download)\
  Other IDEs are not recommended because you need to use certain VS Code extensions (linters and formatters) mentioned in the Coding Style Guide.
* [**PNPM**](https://pnpm.io/installation)
* [**Figma Desktop App**](https://www.figma.com/downloads/)\
  You can alternatively use the [**Figma web app**](https://www.figma.com/) to access Figma in your browser.

### Installing Extensions

At the Coding Style Guide, jump to section 9.1 and install the necessary VS Code extensions. You can also install the optional extensions, but you can do so later.

### Cloning Repositories

There are a couple of repositories that you should clone:

* [**Wangsit Micro-Frontend Boilerplate App**](https://github.com/fewangsit/wangsit-mfe-boilerplate-app)
* [**Wangsit VueJS Component Library**](https://github.com/fewangsit/wangsvue)\
  Clone the Component Library repository to browse the playground folder, where you'll find example implementations of each Vue component. This will be helpful in the later Slicing step. Additionally, you can run the repository to test each component.

Follow these steps to clone the repositories:

### Open the repository pages

Click on the repository links above to go to each repository’s GitHub page.

### Create a new repository from the boilerplate template

For the Boilerplate repository, on the top right of the page, click `Use this template`, and choose `Create a new repository`.\
Note: You need to create it using the GitHub account given to you by the company, not your personal GitHub account.

![](<.gitbook/assets/7607c91b 8c2d 4981 b963 5b53e02c8481.png>)

### Name and privacy

Put in your nickname as the repository name, and make the repository private. Create the repository.

![](<.gitbook/assets/49e97231 0b6f 4e0c b6f1 89ce0b2a58db.png>)

### Copy the clone URL

Once the repository is created, click the `<> Code` button and copy the URL under HTTPS.

![](<.gitbook/assets/c7309d45 1675 47f0 87e5 b6204224a1e9.png>)

### Open folder in VS Code

Open VS Code, and from the toolbar, click `File > Open Folder` to open the folder where you want to clone the repository.



## Open the terminal

Open the terminal with Ctrl + `(backtick, the key above your`Tab\` key). \{% endstep %\}

\{% step %\}

## Clone the repo

Type:

\{% code title="Terminal" %\} \`

\`\`bash git clone

````
Then press Enter and wait for the repository to be cloned.

</div>

</div>

<div data-gb-custom-block data-tag="step">

## Repeat for the Component Library

Repeat the copy-and-clone steps for the Component Library repository.

</div>

</div>

### Installing Packages with PNPM

<div data-gb-custom-block data-tag="stepper">

<div data-gb-custom-block data-tag="step">

## Open the Boilerplate repository in VS Code

In VS Code, open the Boilerplate repository.

</div>

<div data-gb-custom-block data-tag="step">

## Ensure terminal is in the repository root

Open the terminal and make sure it is opened on the root folder of the repository.

</div>

<div data-gb-custom-block data-tag="step">

## Install dependencies

Run:

<div data-gb-custom-block data-tag="code" data-title='Terminal'>

```bash
pnpm i
````

### Wait for installation and read docs

Wait for PNPM to install the packages, which might take a few minutes. While you wait, check sections [4 and 5 of the Code Style Guide](/broken/pages/dbe19b3aa815c0a1bcb4269dc85e71c0c0e9b206#4-project-structure) to understand the repository's structure.

### Restart TypeScript server

After packages finish installing, reload the TypeScript Server by pressing Ctrl + Shift + P and selecting the **TypeScript: Restart TS Server** command.

### Repeat for the Component Library

Repeat the steps above for the Component Library repository.

### Running the Component Library App

{% stepper %}
{% step %}
### Start the dev server

Run the app in development mode:

{% code title="Terminal" %}
```bash
pnpm dev
```
{% endcode %}

You’ll receive a localhost link; click on it to see how your app looks. Vite supports hot module replacement (HMR), so saving files will update the app without full reloads.
{% endstep %}

{% step %}
### Switch playgrounds

By default, the localhost link will redirect you to the Table component’s playground. You can go to other playgrounds by editing the URL or by checking `playground/index.ts`.\
For example, the Button playground URL might be: http://localhost:5173/wangsvue/button
{% endstep %}

{% step %}
### Test component properties

Change code in the playground files to test different properties of each component.
{% endstep %}

{% step %}
### Check component declarations

If you’re unsure what properties mean, check the component’s declaration file.\
Example: `library\components\dropdown\Dropdown.vue.d.ts`
{% endstep %}
{% endstepper %}

### Slicing

Slicing is the process of turning the design provided by the UI/UX designers into code.

{% stepper %}
{% step %}
### Open the Figma file

Open the Learning Kit Figma. If you don't have access yet, ask your mentor. Focus on slicing the content (you don’t need to slice the navigation bar or side bar).
{% endstep %}

{% step %}
### Run the boilerplate app

In your boilerplate repository, run the app with:

{% code title="Terminal" %}
```bash
pnpm dev
```
{% endcode %}
{% endstep %}

{% step %}
### Identify components to use

Identify which component the design uses. For example, the UI below is the [**TabMenu**](https://v3.primevue.org/tabmenu/) component — import the TabMenu component from the Wangsit Component Library and use it in your implementation.

![](<.gitbook/assets/b04ac288 fcac 4f61 8ab5 54a2f13e509e.png>)

If unsure, ask your mentor which component to use.
{% endstep %}

{% step %}
### Aim for pixel-perfect implementation

Use your browser’s DevTools (press F12) and the inspect element tool to measure and match widths, heights, padding, margins, gaps, font sizes, font weights, etc.

![](<.gitbook/assets/6e757f53 1820 4996 ab6c 5fe3b7cc381b.png>)

On Figma, right-click an element and select the layer you want to inspect to see design details.

![](<.gitbook/assets/74e4b76b 07f5 47d3 a7c2 6737bec38382.png>)
{% endstep %}

{% step %}
### Refer to docs

Refer to the [**Official Vue documentation**](https://vuejs.org/guide/introduction.html) and section 6 of the Coding Style Guide to implement features correctly.
{% endstep %}

{% step %}
### Commit and push your feature

After finishing a feature, push it to the repository following the Git Conventional Commits from section 8 of the Coding Style Guide.

Note: While it's okay to push changes to the `main` branch of your personal boilerplate repository, in the actual work environment you should push to a `feat/{feat-name}` branch and then make a pull request to the `dev` branch.
{% endstep %}

{% step %}
### Request review

Ask your mentor to review your work and fix any inconsistencies they point out.
{% endstep %}
{% endstepper %}

***

Updated on December 8, 2025, 8:34 AM UTC

***

Reference: [Vue TypeScript Coding Style Guide - The Standard Coding Pattern](/broken/pages/dbe19b3aa815c0a1bcb4269dc85e71c0c0e9b206)

![](<.gitbook/assets/7607c91b 8c2d 4981 b963 5b53e02c8481.png>)

![](<.gitbook/assets/49e97231 0b6f 4e0c b6f1 89ce0b2a58db.png>)

![](<.gitbook/assets/c7309d45 1675 47f0 87e5 b6204224a1e9.png>)
