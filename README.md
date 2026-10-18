# Navi Dev Kit

A small s&box game that contains the [Navi](https://github.com/Fractitude/Navi) library and a ready-made Navi desktop. Nothing else: no gameplay and no other game's assets.

It is published on the workshop as `fractitude.navi_devkit`. Its purpose is to be the **parent package** of a Navi app's addon project, so that app developers can run and test their app on a Navi desktop while they work on it, without publishing it first and without any game's source code.

## Test an app in your addon

1. In the addon's `.sbproj` (and in the `.napp`'s `.meta` file, under `Metadata`), set `ParentPackage` to `fractitude.navi_devkit`. In the editor this is Project Settings, Target Game. Restart the editor.
2. Put the Navi desktop in your scene (see below).
3. Add your app to the computer's **Installed Apps**: select the `NaviComputer` component and add your `.napp`'s path, for example `myapp.napp`.
4. Press Play and click the screen. The first click sits you at the computer.

Don't install the Navi library in the addon: the published app would then bundle its own copy of Navi. The dev kit brings it, and it stays out of your package.

How to write the app and publish it is in the Navi library's README, "Make an app" and "Publish an app to the Store".

## Set up the scene

Open `Assets/Navi Desktop.prefab` to see the whole setup. It is one GameObject with four components:

- `WorldPanel`: the screen, in the world.
- `NaviDesktop`: draws the desktop, windows and taskbar.
- `NaviComputer`: the OS. `Installed Apps` lists the apps that are installed from the start; Packages and Settings are core apps and are always installed.
- `NaviInputCapture`: takes the player's keys while they sit at the computer.

To set up your own scene:

1. Add the **Navi Desktop** prefab to the scene, or build the same GameObject by hand: add a `WorldPanel`, then a `NaviDesktop` (it adds the other two). Place it where the camera can see it, within the world panel's `InteractionRange`.
2. Add a **`WorldInput`** component to the scene's camera. This is the only other thing needed: it lets the mouse click the world panel.

`Assets/navi_devkit.scene` is a small scene with a camera, a player and the desktop already set up.

The same two steps work in any other game or project that has the Navi library installed.

## What is in this repository

- `Libraries/navi`: the Navi library, as a git submodule of [Fractitude/Navi](https://github.com/Fractitude/Navi). Clone with `--recurse-submodules`.
- `Assets/`: the Navi Desktop prefab and the example scene.
- `navi_devkit.sbproj`: the project, `Org` `fractitude`, `Ident` `navi_devkit`.

## Publishing the dev kit

The dev kit's package contains its own copy of Navi, built from `Libraries/navi`. To update it: update the submodule to the Navi you want, check that the project compiles, and publish with the editor's normal Publish. An app's addon loads the dev kit's version of Navi, so republish the dev kit whenever Navi changes in a way apps should see.
