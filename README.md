# o2-banners-SMB-Winter-Trading-2020
Trello: https://trello.com/b/2zpApSRs/o2ukl3123e-o2ukl3123h-o2-smb-q4-black-friday-winter-trading

Easy start
----------

  npm install
  gulp serve
  gulp build

- open a folder of the phase you want to work with
- run `npm install` once, to install stuff
- run `gulp serve` or `npm run dev`
- `src/` is your working directory
- run `gulp build` or `npm run build` to build final ZIP files
- `zip/` contains FlashTalking ready ZIP files
- `0.0.0.0:8080/preview` will open in your browser automatically
- if you need to supply for EG+, run `gulp eg` or `npm run eg`

Gulp
----

- gulp takes care of preparing the files for FlashTalking. You don't have to change, rename or ZIP anything manually
- gulp also takes care of vendor prefixes, concatination, minification, tests your JS, etc
- gulp builds all banners only once, then just the banner you are working on
- gulp overwrites everything in these folders: `./dist`, `./tmp`, `./zip` - so __don't edit them directly__

Handlebars
----------

- in several files, you will find {{width}}, {{height}}, {{richload}}, {{api}} and {{deploy}} strings, where numbers or strings should be
- please don't remove them, they will save you a lot of time
- {{width}} and {{height}} are based on the folder name
- {{build}} is an identifier, that is generated each time grunt runs (to avoid cache, mostly)
- {{api}} is stored in `package.json` file
- {{richload}} stores the Richload ZIP filename, and is needed in the `manifest.js` only
- {{debugMode}} This property is related to console.log function, in development you will be able to print console log messages, in zip/richload builds console log is automatically removed so Flash Talking doesn't reject.

Handlebar | Note
--- | ---
{{width}} | width of the banner, determined by folder name
{{height}} | height of the banner, determined by folder name
{{build}} | identifier of build, generated for each grunt run, helps to avoid cache
{{api}} | API setting in grunt config
{{richload}} | richload generated file name (without the .zip extension)
{{git.origin}} | GIT project name
{{git.branch}} | GIT branch
{{git.author}} | Initials of the author of the build
{{git.time}} | Time of build
{{buildName}} | `banner-{{build}}` shorthand
{{test}} | used in html comment condition. In `preview` version, the {{test}} is replaced with empty string, but in `dist` version, the space is added
{{manifest-setup}} | basic manifest setup
{{manifest-setup-eg}} | eg manifest setup

Folders and automatic builds
----------------------------

- The folder structure in `src` determines the resulting ZIP file name
- The folder containing the `richload.html` file should have the name of __WIDTHxHEIGHT__, like '300x250'. This value is then used to populate all handlebars {{width}} or {{height}} in `manifest.js` or `richload.html` file
- This also means, __to create a new size, just duplicate the folder and rename it__ to the size you need. No fiddling with gruntfile or package.json
- my_new_folder/300x250 results in `my_new_folder_300x250.zip`
- there are also `manifest.header.html` and `manifest.footer.html` files. This will wrap around your `richload.html` file.
- in the `./shared/eg/` there are EG+ specific overrides
- to override any file from `shared` folder, simply copy it into your banner and amend, as needed

CSS and JS
----------

- don't worry about vendor prefixes. These are done automatically, IE9 and up
- don't edit CSS files directly, edit SCSS files instead. If you don't know these, learn about SCSS, they are very similar, but save a ton of time
- JS concats everything, what is specified in the `./shared/richload.footer.html`
- to speed up things, a version of FlashTalking Library is stored in `./shared/js-dev-only` folder. This JS doesn't get into the final JS build, but saves time, because while building, you don't have to wait for flashtalking servers every time
- there are default versions of `ad-` files in `./shared`. Creating a file carring the same name will override the default file before concatenation
- usually you just override two files: `ad-animation.js`
- usemin lets you determine, which files should make it into the final build. But usually you won't need to change this
- not all SCSS is in `this.scss` file, generic repeating classes and definitions are stored in `./shared/css/core.scss`
- you can use `$width` and `$height` values in any SCSS file


Images
------

- there are two types of images in the build. Static ones (like the O2 logo) and Dynamic ones (like product image)
- static ones can be put into each individual `./src/Banner/300x250/img` folder or in the shared `./shared/img` folder
- static images are zipped into the **Richload**
- dynamic ones, that are called as FT variable, should be placed into the root `./assets` folder
- in the `manifest.js` file, you then reference it as `/assets/image.png`
- to simplify writing dynamic links for different sizes, you can use handlebars '/assets/image-{{width}}x{{height}}.png'
- by doing this, after you upload your **IMAGES** from `assets`, **RICHLOAD** and **BASE** file to FT, they get automatically linked and you don't have to do anything to link them again
- **!! remember** to upload images from `assets` folder, before uploading **BASE** files to FT
- **!! caution** new FT doesn't allow empty string as image link in `manifest.js`. That's why there is a `empty.png` in `assets` and `manifest.js`

Starting from scratch?
----------------------

This is the most basic folder structure, that get's you underway. You don't even have to use any handlebars, everything is just up to you. And you never have to touch `gruntfile.js` or `package.json`:

  - src
    - my_banner_campaign_name
      - 300x250
        - css
          - this.scss
        - js
            - ad-animation.js
            - ad-bubble-settings.js
          - this.js
        - img
          - anyimage.png
        - richload.html
        - manifest.js

About manifest file
-------------------

- !! __Remember to upload assets and richload before uploading base__

- This manifest file offers many variables for most of your banner needs. You might not need all of those. But you can utilise some extra dynamic properties in {{handlebars}}

- There are some basic defaults, for text and image variables in new FT:
  - text: can be an empty string - ""
  - image: cannot be empty, so we use `"/assets/empty.png"` in assets folder

- if the path to spritesheet doesn't match "/assets/empty.png", spritesheet will be used 
- if the first two "slide" text variables are not empty, carousel will be used
- bubbles are always used, expect in 300x50 and 320x50 banners (performance)

- The {{width} and {{height}} variable cen help to avoid manual editing of filenames, that have the banner dimension in their name.
- Instead of typing 970x250-product.png you can use a global `{{width}}x{{height}}-product.png`, which will be compiled into `970x250-product.png`

Variable | Type | About | Note
--- | --- | --- | ---
iRichload | richload | richload name file | dynamically created
img_1 | image | basic image variable | use numbers up to image_5
copy_1 | text | basic text variable | use numbers up to copy_5
endframe_title | text | endframe main copy
endframe_copy | text | endframe body copy
endframe_note | text | endframe note copy
endframe_product | image | product used on endframe
cta | text | button copy
terms | text | usually just a "terms apply" copy
CSS | text | creates a `<style>` element
JS | text | creates a `<string>` element
URL | text | override clicktag setting
SMB | text | determines, if the banner is business or carousel (selects logo) | `true` or `false`
spritesheet_path | image | path to spritesheet file, if empty, spritesheet is not used
spritesheet_frame_width | text (number) | spritesheet frame width | recommended {{width}}
spritesheet_frame_height | text (number) | spritesheet frame height | recommended {{height}}
spritesheet_frames_per_line | text (number) | number of frames per line
spritesheet_frames_total | text (number) | number of frames total
spritesheet_loop | text (number) | amount of time the spritesheet should play

Using FT variables in HTML
--------------------------

- you don't have to do any manual assignement in JS
- you only have to use the variable name from FT as an attribute `data-ft="variable-name"` with any element

Variable type in FT | Element tag in HTML | Example | Result
--- | --- | ---
image | `<IMG>` | `<img data-ft="VARIABLE">` | `<img src="CONTENT">`
image | not `<IMG>` | `<div data-ft="VARIABLE"></div>` | `<div style="background-image: url(CONTENT)></div>`
text | any <TAG> | `<div data-ft="VARIABLE"></div>` | `<div>CONTENT</div>`

Using FT variables in JS
------------------------

- if you ever need to access FT variables, use `Advert.instantAds['variable-name']` anywhere in the script
