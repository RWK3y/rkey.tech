---
title: "Build RKey Tech"
date: 2022-03-02T06:31:37-07:00
draft: false
description: "Build rkey.tech project"
---
[Matching Blog Post](/posts/buildopekkttech)

Create new Hugo project
```
bsh ➜ hugo new site rkey.tech
```
cd into new project and initialize
```
bsh ➜ cd rkey.tech
bsh ➜ git init
```
Verify and initialize 'go'
```
bsh ➜  go version
go version go1.17.7 linux/amd64
bsh ➜  hugo mod init rkey.tech

```
I initially tried to use the recommended method for the Congo theme which was to create a module.toml file under config/_default and start the Hugo server. This appeared to work and the screen even said it was pulling the repository but after starting Hugo server I had no themes/congo directory or files. Maybe this is normal and it pulls the repository each time Hugo starts, but being a gnubie with Hugo I like to feel and touch my files, even if they are just for theming. So instead I went the submodule route, which is what I have been doing with all previous themes.

```
bsh ➜  git submodule add -b stable https://github.com/jpanther/congo.git themes/congo
bsh ➜  cd themes/congo/_default
bsh ➜  cp config.toml languages.en.toml markup.toml menus.en.toml params.toml rkey.tech/config/_default/
bsh ➜  cd config/_default
bsh ➜  vi config.toml
```
Added "theme = "congo" to the top of the file since I'm using the submodule method.
```
bsh ➜  cat config.toml
# -- Site Configuration --
# Refer to the theme docs for more details about each of these parameters.
# https://jpanther.github.io/congo/docs/getting-started/

theme = "congo"
```

I left the module.toml file in _defaults as below, hopefully it does not cause any issues. If so I can always delete it later.
```
bsh ➜  cat module.toml
[[imports]]
path = "github.com/jpanther/congo/v2"
```
Then modified languages.en.toml adding domain name and my logos and images for the site.  Also added social links.
```
bsh ➜  cat languages.en.toml
title = "RKey Tech"
 logo = "img/rkeytechno.png"
 description = "RKey Tech Home"

[author]
   name = "Robyn Key"
   image = "img/icon-foreground.png"
```
I initially put my images and logos in static/images as I did with other themes, but with this Congo theme the standard is assets/img so I created the img folder and copied my images there.

Modified params.toml and made the following changes from defaults
```
bsh ➜  cat params.toml
# -- Theme Options --
# These options control how the theme functions and allow you to
# customize the display of your website.
colorScheme = "slate"
defaultAppearance = "dark" # valid options: light or dark
autoSwitchAppearance = true
showAppearanceSwitcher = false
enableSearch = true
enableCodeCopy = true
[homepage]
  layout = "page" # valid options: page, profile, custom
  showRecent = false
   sharingLinks = ["facebook", "twitter", "pinterest", "reddit", "linkedin", "email"]
```
I then modified the menus.en.toml. I like for my tags to be displayed in the footer not the header.
```
bsh ➜  cat menus.en.toml
# -- Main Menu --
# The main menu is displayed in the header at the top of the page.
# Acceptable parameters are name, pageRef, page, url, title, weight.
#
# The simplest menu configuration is to provide:
#   name = The name to be displayed for this menu link
#   pageRef = The identifier of the page or section to link to
#
# By default the menu is ordered alphabetically. This can be
# overridden by providing a weight value. The menu will then be
# ordered by weight from lowest to highest.

[[main]]
  name = "About"
  pageRef = "about"
  weight = 10

[[main]]
  name = "Docs"
  pageRef = "docs"
  weight = 20

[[main]]
  name = "Blog"
  pageRef = "posts"
  weight = 30

[[main]]
  name = "External_Sites"
  pageRef = "ext"
  weight = 40

# -- Footer Menu --
# The footer menu is displayed at the bottom of the page, just before
# the copyright notice. Configure as per the main menu above.

 [[footer]]
   name = "Categories"
   pageRef = "categories"
   weight = 10

 [[footer]]
   name = "Tags"
   pageRef = "tags"
   weight = 20
```
Another thing different in Congo that I'm not used to is the location of FavIcons.  In Congo they go directly under static. For all my favicons, I use <a href="https://cthedot.de/icongen/" target="_blank">IconGen</a> as you can see from my listing below, I think I covered my bases, though I'm certain I'm wasting resources being this thorough.
```
bsh ➜  ls static
android-144x144.png                  apple-touch-icon.png  icon.png
android-192x192.png                  browserconfig.xml     msapplication-icon-144x144.png
android-36x36.png                    coast-228x228.png     mstile-144x144.png
android-48x48.png                    favicon-128x128.png   mstile-150x150.png
android-72x72.png                    favicon-16x16.png     splash.png
android-96x96.png                    favicon-256x256.png   tile150x150.png
android-chrome-192x192.png           favicon-32x32.png     tile310x150.png
android-chrome-512x512.png           favicon-48x48.png     tile310x310.png
android-chrome-maskable-192x192.png  favicon-64x64.png     tile70x70.png
android-chrome-maskable-512x512.png  favicon-96x96.png     Windows10
apple-touch-icon-120x120.png         favicon.ico           Windows10store
apple-touch-icon-152x152.png         FirefoxOS             WindowsApp.Shared
apple-touch-icon-167x167.png         _head.html            WindowsApp.Windows
apple-touch-icon-60x60.png           icon-background.png   WindowsApp.WindowsPhone
apple-touch-icon-76x76.png           icon-foreground.png
```
I wanted to get rid of the 'Powered by Hugo and Congo` in the footer` and only have the copyright and site links in the footer.  So I copied `themes/congo/layouts/ to layouts`. So I would have `layouts/partials` in my own namespace instead of the theme's.
```
bsh ➜  cp -a themes/congo/layouts/* layouts/
```
I then modified layouts/partials/footer.html and changed
```
      {{/* Theme attribution */}}
      {{ if .Site.Params.attribution | default true }}
```
To
```
      {{/* Theme attribution */}}
      {{ if .Site.Params.attribution | default false }}
```
Then I created layouts/partials/extend-footer.html with the following:
```
bsh ➜  cat extend-footer.html
<p class="text-sm text-neutral-500 dark:text-neutral-400">For <a href="/">RKey.Tech</a> and
<a href="https://r0bwk3y.com" target="_blank">R0bWK3y.com</a>.</p>
```
The main thing to make it match the Congo theme was adding
```
<p class="text-sm text-neutral-500 dark:text-neutral-400">
```
I tried to do a custom 404 page, but have not been able to figure that one out with the Congo theme. The standard way of using 404.md, 404.html in layouts/ and even putting a complete 404.html in content/ does not work. But honestly the function built into Congo is so elegant I think I like it better anyway

Stay tuned for more. Next will be ~~setting up repository in GitLab~~, setting up mirroring from GitLab to GitHub, ~~deploying servers on Vultr~~, ~~migrating some services from Digital Ocean to Vultr~~, securing the new servers, ~~installing jails and services~~ and finally ~~switching domains over~~.
