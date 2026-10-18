# Navi Dev Kit

A small s&box game that contains the [Navi](https://github.com/Fractitude/Navi) library and a ready-made Navi desktop. Nothing else: no gameplay and no other game's assets.

It is published on the workshop as `fractitude.navi_devkit`. Its purpose is to be the **parent package** of a Navi app's addon project, so that app developers can run and test their app on a Navi desktop while they work on it, without publishing it first and without any game's source code.

## Develop an app with the dev kit

An app lives in its own **addon** project. The dev kit is that addon's parent: it supplies the Navi library and a ready-made computer, so you can run the app while you write it.

### 1. Create the addon

1. Open the s&box editor and create a new project (the start window's **New Project**, or the editor's File menu).
2. Set the type to **Addon**. Give it a title, an **Org** (your s&box organization) and an **Ident**, for example `myapp`. The ident is the project's name, not the app's: the app gets its own ident, `napp_myapp`, when you publish it.
3. Pick a folder and create it. The editor opens the new project and compiles it.

The project has a `Code/` folder for your C# and Razor files and an `Assets/` folder for everything else. Both are compiled and hot-reloaded by the editor as you save.

### 2. Make the dev kit its parent

1. In the editor open **Project Settings** (the menu bar's Project, then Project Settings).
2. Find **Target Game** (stored as `ParentPackage`) and set it to `fractitude.navi_devkit`. The editor downloads the dev kit.
3. Restart the editor when it asks, or close and reopen the project.

Or edit the project's `.sbproj` while the editor is closed, and add this under `Metadata`:

```json
"ParentPackage": "fractitude.navi_devkit"
```

Don't install the Navi library in the addon yourself: the published app would then bundle its own copy of Navi. The dev kit brings it, and it stays out of your package.

### 3. Put the computer in a scene

1. In the Asset Browser, open the **Navi Dev Kit** package in the left-hand list. The dev kit's files are listed under it.
2. Find **`navi desktop.prefab_c`** (shown as `Navi Desktop`).
3. Open (or create) a scene in your addon and **drag the prefab into it**. It is the whole computer: you set nothing up.
4. Add a **`WorldInput`** component to the scene's camera. This is the one thing the prefab can't bring, and it lets the mouse click the screen.

Place the computer where the camera can see it. Its screen is clickable only from within its `WorldPanel`'s `Interaction Range` (100 units by default), so stand close or raise the range. The prefab is explained under "Set up the scene" below.

### 4. Install your app on the computer

Your app is a `.napp` file in your addon's `Assets/` (see the app guide for how to make one). To have the computer start with it installed:

1. Select the Navi Desktop in the scene and find the **`Navi Computer`** component in the inspector.
2. Open its **Installed Apps** list and add one entry: your `.napp`'s path, relative to `Assets/`. For `Assets/myapp.napp` that is `myapp.napp`. For `Assets/apps/myapp.napp` it is `apps/myapp.napp`.

To find the path: in the Asset Browser right-click the `.napp`, choose **Copy Relative Path**, and paste it. It must match exactly, including the folder and the `.napp` extension. An entry that matches nothing is skipped quietly, so if your app isn't there, check the path first.

Packages and Settings are core apps and are always installed. The other pre-packaged apps (Calculator and so on) appear only if you add them to the list too, as `navi/apps/calculator.napp`.

### 5. Run it

1. Press **Play** and click the screen. The first click sits you at the computer.
2. Open the start menu in the taskbar. Your app is in the list.
3. Change your code. The editor hot-reloads it; reopen the app's window to see the change. If an edit doesn't show, stop and press Play again.

How to write the app is in its own guide. How to publish it is in the Navi library's README, "Publish an app to the Store".

## Set up the scene

You don't need this to use the prefab. Open `Assets/Navi Desktop.prefab` to see the whole setup. It is one GameObject with four components:

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
