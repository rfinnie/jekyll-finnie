---
id: 1777
title: Hampr extension and Firefox 4.0
date: 2011-03-25T16:20:50+00:00
author: Ryan Finnie
layout: post
guid: http://www.finnie.org/?p=1777
permalink: /2011/03/25/hampr-extension-and-firefox-4-0/
categories:
  - Hampr
tags:
  - planet:canonical
---
Firefox 4.0 was released this week, and while the Hampr extension is technically compatible with it out of the box, it's not ideal. Firefox 4.0 for Windows defaults to hiding the menu bar in favor of an "app menu", a dropdown next to the tab bar. Because the Hampr extension's default mode (on Windows and Linux) is to replace the Bookmarks menu with the Hampr menu, the user will never see the Hampr interface unless they've changed the default Firefox behavior and are showing the main menu bar.

I will go into some more detail about the tech below, but for the impatient, there are one of two things you can do immediately to fix this:

A) Enable the main menu bar.

  1. Click "Firefox", hover over "Options", and click "Menu Bar".
  2. Hampr will be waiting for you on the main menu bar, no restart necessary.

B) Switch the Hampr extension to "Toolbar button" mode.

  1. Click "Firefox", then "Add-ons", then "Extensions". Click "Options" on the line for Hampr.
  2. For "Display mode", select "Toolbar button". Click "OK" for the restart warning, then "OK' for the options dialog.
  3. Click "Firefox", hover over "Options", and click "Toolbar layout".
  4. Drag the Hampr button (which looks like a yellow folder) to a convenient location, for example next to the address/search bar.
  5. Click "Done" and restart Firefox.

Hampr has two more modes, "Add to main menu bar" and "Add to bookmarks menu", but unfortunately both are made useless by Firefox hiding the main menu bar. The first is obvious, but "Add to bookmarks menu" would seem useful, since the Firefox bookmarks menu is shown in four locations, depending on layout:

  * From the main menu bar, if the main menu bar is visible.
  * From the app menu, if the main menu bar is hidden.
  * From the bookmarks dropdown button on the address bar, if the main menu bar is hidden.
  * From the bookmarks toolbar, if the bookmarks toolbar is visible and the main menu bar is hidden.

The problem is, while they all look identical, they're actually four completely separate copies as far as Firefox is concerned. The Hampr extension only knows how to hook into the bookmarks menu of the main menu bar, so "Add to bookmarks menu" only displays on the main menu bar's bookmarks menu.

I will look into adding new functionality to hook into these menus, but it will probably take a lot more code, so it's not going to be immediately. In the meantime, enabling the main menu bar is probably the easiest choice if you are an avid Hampr user.

(Firefox 4.0 for Linux doesn't seem to default to hiding the main menu bar. And due to severe technical restrictions with OS X (namely, a menu bar dropdown cannot be changed while it is open), main menu bar integration was never available for Mac to begin with.)