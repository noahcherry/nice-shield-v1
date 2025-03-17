# nice-shield-base

> [!IMPORTANT]
> WIP

This is a base repo to help anyone get started creating their own images/animations for their nice!view.


> [!IMPORTANT]
> This repo is only for nice!views and zmk with zephyr modules. This will NOT work for OLED/qmk setups.
> Since this is a zephyr modules(you dont really need to know what this means), anyone will be able to use your images since the repo is hosted on github.
>
> If you would like OLED/qmk examples, check out my collection of OLED displays for qmk [HERE](https://github.com/whoop-t/qmk-oled-collection)

> [!IMPORTANT]
> When you have created your own display, consider adding it to this collection I started for nice!view displays!
> [nice shield collection](https://github.com/whoop-t/nice-shield-collection)

## Fork or clone the repo

> [!IMPORTANT]
> A convention I like to do is to name the repo/module starting with `nice` to signify that this is a nice!view module.
> e.g if I was creating displays with star art, I'd name it something like `nice-star-display` or something similar.


### Fork

Fork repo through github

Then clone the repo to your local computer to start work

### Clone the repo(RECOMMENDED)
```
git clone git@github.com:whoop-t/nice-shield-base.git your-repo-name
```
The above will clone the base repo into a folder of your choice. This should match the repo name you are going to create on github

2. Create new repo on github

On you github, create a new repo with the same name as your folder you cloned too

3. Set your locally cloned repo remote to the new repo you created on github

You need to attach your local clone to the remote repo, do this by running the command below
> [!NOTE]
> You can get the url by using the Code button on your repo on github 
```
git remote set-url origin git@github.com:YOUR-USER/your-repo-name.git
```

You can verify that it is pointing to the remote repo with `git remote -v`

4. Push the local files up to the remote repo
```
git push
```
Now you should see all the base files in your repo on github

## Renaming

After you have the code, you will need to rename some items to make sure it gets picked up as a module when it is used

1. Rename ids/variables in the code

In `Kconfig.shield`, update `nice_shield_base` to your shield
This will be your repo name but with underscores

> [!IMPORTANT]
> Do NOT update `SHIELD_NICE_VIEW_SHIELD_BASE`, only `nice_shield_base` in the parenthesis
```
config SHIELD_NICE_VIEW_SHIELD_BASE
    def_bool $(shields_list_contains,nice_shield_base)
```

> [!IMPORTANT]
> `id:` should be hyphen case, eg if you repo is `your-repo`, make sure you use hyphens for the id

In `shields/nice_shield_base/nice_shield_base.zmk.yml` rename these to your repo name:
```yaml
id: nice-shield-base
name: nice!view_shield_base
```

In `zephyr/module.yml` rename this to your repo name:
```yaml
name: "zmk-shield-nice!view-your-repo"
```
> [!NOTE]
> I dont think this one matters, but I like to update it

2. Rename files
> [!IMPORTANT]
> Files should be underscore

- Rename `shields/nice_shield_base` to your repo name
- Rename `shields/nice_shield_base/nice_shield_base.conf` to your repo name
- Rename `shields/nice_shield_base/nice_shield_base.overlay` to your repo name
- Rename `shields/nice_shield_base/nice_shield_base.zmk.yml` to your repo name

## Test your repo!!!

After all the renaming and hooking up to your repo, test everything is working correctly by adding your new repo to your ZMK config.

The repo already has a base image that can be flashed and tested before adding your own art to make sure the steps were followed correctly.

### Add your new module to your ZMK config

Add your remote repo to `west.yaml`
```yaml
manifest:
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    - name: your-github-user #new entry
      url-base: https://github.com/your-github-user #new entry
  projects:
    - name: zmk
      remote: zmkfirmware
      revision: main
      import: app/west.yml
    - name: your-repo-name #new entry
      remote: your-github-user #new entry
      revision: main #new entry
  self:
    path: config
```

Add your module to `build.yaml`(this is for corne, but change for your keyboard if needed)
```yaml
include:
  - board: nice_nano_v2
    shield: corne_left nice_view_adapter your_repo_name #update entry
  - board: nice_nano_v2
    shield: corne_right nice_view_adapter your_repo_name #update entry
```

Also make sure to enable the custom status screen in your ZMK configuration, this would be your keyboards .conf file in the config directory:

```
CONFIG_ZMK_DISPLAY=y
CONFIG_ZMK_DISPLAY_STATUS_SCREEN_CUSTOM=y
```

Now flash your board and you should see the base image from this repo. If there is an issue, go back and walkthrough the steps again and make sure you did everything correctly.

## Create 1-bit art
> [!IMPORTANT]
> ART MUST BE 80x69
>
> You can alter this in the code where you define your image so you can technically change this and make your art a different size, but this is the size I have found to be the best while keeping other information like battery etc

> [!IMPORTANT]
> WIP
> TODO Add guide to create the pixel art

> [!IMPORTANT]
> WIP
> TODO Add guide to animations and set in repo

## Add images

> [!NOTE]
> If you want animations, you can skip to the animation section
> This section is just for still images

> [!NOTE]
> This section also assumes you already have an image created from the previous step

### Rotate your image

This is a horizontal display so you need to rotate the image 90 degrees to the right

I just use [iloveimg](https://www.iloveimg.com/rotate-image), but feel free to use anything. Just make sure you are rotating it 90 degrees to the right

### Image to code conversion

Take your 1-bit art that your created or found and upload it to
[image2cpp](https://javl.github.io/image2cpp/)

The only option you need to change is `Code output format`, make sure its set to `Plain bytes`

Then click `Generate Code`
You will see the code, copy everything in the textfield

## Paste to code in repo

Depending on the image you are setting, you will need to update on of the following files:
1. `assests/left_image.c` (for your left half)
2. `assests/right_image.c` (for your right half)

You should see bytes already there, as well as a comments in the code above and below the bytes
Replace the bytes with your bytes from the conversion
```c
  // REPLACE THESE BYTES
  ...
  ...
  // END
```

Save the file

Thats it! Repeat for your other side if needed

## Push the code to you github remote repo

Once you have done all the changes you want, push the code to your remote github repo
```
git add .
git commit -m "Your commit message"
git push
```

Then you just need to adjust your ZMK config to use the module like in the above step and flash your board!

## Issues

If you have issues, raise an issue on github and I will try to help out when I have time. This will also help me tweak the guide with anything I may have missed.
