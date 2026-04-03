---
title: Getting started
slug: getting-started
weight: 1
---
To create a site with Osuny, the simplest solution is to generate it based on the admin, which uses the [template](https://github.com/osunyorg/template).
This template uses the [osuny theme](https://github.com/osunyorg/theme).

The admin template offers an up-to-date site configuration. It contains Hugo, the theme and scripts that facilitate updates.
As the sites are generated with Hugo, the code must be installed locally.

This happens automatically when you create the site in the Osuny admin interface, you just need to clone the repository.

> Before the AAA template, there was another template, called [osuny-hugo-template](https://github.com/noesya/osuny-hugo-template), which used the theme [osuny-hugo-theme](https://github.com/noesya/osuny-hugo-theme). This template and theme are obsolete. It was subject to several redisigns, and itself succeeded the Jekyll theme, at the beginning of Osuny. The AAA reference refers to the article [Qualité frontend : à la recherche du AAA](https://lab.noesya.coop/publications/2022-05-09-qualite-frontend/), published on [Lab noesya](https://lab.noesya.coop).

## Install Hugo

> ⚠ The theme currently supports the 0.157.0 version of hugo. See [modifier la version d'Hugo.io](/docs/website/sujets-avances/modifier-la-version-d-hugo/)

{{< tabs items="MAC,PC" >}}
  {{< tab >}}
    On Mac, with [Homebrew](https://brew.sh), use the command:

    ```bash
    brew install hugo
    ```

    This is the method we use in the [noesya](https://www.noesya.coop) team.
    For other methods, the [official installation documentation](https://gohugo.io/getting-started/installing/) is available at [gohugo.io](https://gohugo.io).
  {{< /tab >}}
  {{< tab >}}
    It is also possible to install Hugo with the [mise](https://mise.jdx.dev/) tool, a manager version for a multitude of tools like Ruby, Node, etc.

    To install Mise:
    ```bash
    curl https://mise.run
    ````

    Once installed, to install a complete version of Hugo (e.g. 0.157.0):
    ````
    mise use -g hugo-extended@0.157.0
    ````
  {{</tab>}}
  {{< tab >}}
    The easiest way to install Hugo on Windows is to use a package manager, such as [Scoop](https://scoop.sh) or [Chocolaty](https://chocolaty.org).

    ```bash
    choco install hugo-extended
    ```

    See [official installation documentation](https://gohigo.io/installation/windows/) for more information on the installation with Windows.
  {{< /tab >}}
{{</tabs>}}

## Install Yarn

{{<tabs items="MAC,PC">}}
  {{<tab>}}
    On Mac, with [Homebrew](https://brew.sh), use the command:

    ```bash
    brew install yarn
    ```

    [official installation documentation](https://yarnpkg.com/getting-started/install).
  {{</tab>}}
  {{<tab>}}
    To install Yarn on Windows, the recommended method is to use NPM included with Node.js installation.
    ```bash
    npm install --global yarn
    ```
    For more information on installing Yarn on Windows, see [official installation documentation] (https://classic.yarnpkg.com/en/docs/install).
  {{</tab>}}
{{</tabs>}}

## Clone the Git Repository

The Git repositories are created by Osuny, there is no longer a need to create them by hand.
If you did need to do this by hand, you would start from [the template page](https://github.com/noesya/osuny-hugo-template-AAA).
Click on the "Use this template" button, then give a name and validate.
In this tutorial we will use the name `my-project`.

Once the repository is created, it must be cloned locally.
The theme is a git submodule.
To clone with the theme, use the command:

```bash
git clone git@github.com:my-organisation/my-project.git --recurse-submodules
```

> If you have forgotten `--recurse-submodules` at the time of the clone, you can add the theme afterward:
> 
> ```bash
> git clone git@github.com:osunyorg/theme.git --recurse-submodules
> cd theme
> yarn
> hugo server
> ```

## Run the server

The first time, we install dependencies:
```bash
cd my-project
yarn install
```

To run the server, we use the command:

```bash
yarn osuny dev
```
<!--
## Using Example Data

You can use sample data, presenting all possible cases with Osuny, which allows you to work on the appearance of the site even before you have published content. Of course, you can simply go back to your actual data as soon as it is available, and alternate according to your needs.

To install the sample content, the command is used:

``bash
yarn osuny setup-example
````

To work on the site with the sample content, we use the command:

``bash
yarn osuny server-example
````

WARNING: Something doesn't work with this command, you have to fix it. -->

## What now?

The general idea to develop your site based on Osuny is to proceed by following the following steps.

{{< callout type="warning" >}}
We don't touch the theme `osuny`!

All modifications to create a site are made in the repo of the site, there is never a need to modify the theme itself (`themes/osuny`).
{{</callout>}}

### 1. Hugo configuration

{{< filetree/container >}}
  {{< filetree/folder name="config" >}}
    {{< filetree/folder name="_default" >}}
      {{< filetree/file name="config.yaml" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

First, try to configure everything using the file `/config/_default/config.yaml`.

This allows you, for example, to define the position of the breadcrumb trail, the summary, the truncation length, or to choose between a list or grid layout for the news items.
When something is not customizable in the config.yaml file, you proceed to step 2. The file is empty; you must find the variables in the documentation or in the theme itself and paste them into the file.

You can find available variables in:
https://github.com/osunyorg/theme/blob/main/hugo.yaml


### 2. SASS configuration 

{{< filetree/container >}}
  {{< filetree/folder name="assets" >}}
    {{< filetree/folder name="sass" >}}
      {{< filetree/file name="_configuration.sass" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

https://github.com/osunyorg/theme/blob/main/assets/sass/_theme/_configuration.sass
The file `/assets/sass/_configuration.sass` is used to define variables that will be used by the theme.

The available variables are listed here:
https://github.com/osunyorg/theme/blob/main/assets/sass/_theme/_configuration.sass

### 3. SASS Style personalization

{{< filetree/container >}}
  {{< filetree/folder name="assets" >}}
    {{< filetree/folder name="sass" >}}
      {{< filetree/file name="_style.sass" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

When a modification cannot be made using variables, you must write Sass code in the `/assets/sass/_style.sass` file.

To write CSS selectors, you can use the DOM or consult the theme files.
Whenever possible, use the theme's helpers and conventions (`px2rem(20)`, `@include media-breakpoint-up(desktop)`, etc.).
This helps maintain consistency and avoid side effects, especially those related to responsive design.


### 4. HTML Overrides

{{< filetree/container >}}
  {{< filetree/folder name="layouts" >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

Finally, when styling alone isn't enough, all the HTML markup can be modified by duplicating the theme files in the `/layouts` folder.

Note that this should only be done as a last resort, because doing so will prevent you from receiving theme updates.
When the theme evolves, you must update your own HTML files to maintain compatibility.
Of course, your HTML modifications must take into account accessibility and clean design principles in the same way as the theme.
