# MyBlog

Create using [hugo](https://gohugo.io/) and [Pure](https://themes.gohugo.io/themes/hugo-theme-pure) template.

## Commands to create this repo
```
hugo new site myblog

git init

git submodule add https://github.com/cofess/hexo-theme-pure.git themes/pure
```
## Commands to create new blog

```
hugo new posts/<post-name.md>
```

## Start serve

```
hugo server -D
```

## Blog settings

The setting of this blog were modified from the `exampleSite` comes with the "fuji" theme.

## Workflow setup

Follow this [guide](https://github.com/marketplace/actions/hugo-setup) to setup the workflow to deploy the hugo static site.