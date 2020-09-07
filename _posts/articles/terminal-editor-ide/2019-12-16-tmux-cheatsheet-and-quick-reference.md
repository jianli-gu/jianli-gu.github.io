---
layout: post
title: Tmux Cheatsheet and Quick Reference
categories: [Terminal & Editor & IDE]
tags: [Tmux]
modified: 2019-12-16
---

**Table of Contents**

1. TOC
{: toc}

## Introduction

Tmux has three levels of hierarchy: Session, window, and pane.
* *Session*: A group of windows, plus a notion of which window is current.
* *Window*: A single screen with a group of panes.
* *Pane*: A rectangular part of a window that runs specific commands.

`Ctrl + b` is the *prefix key*.

## Sessions

* Start new session

    ```bash
    $ tmux
    $ tmux new
    $ tmux new-session

    $ tmux new -s mysession
    ```

    Or use command mode,

    ```bash
    : new
    : new -s mysession
    ```

* Show sessions
  
    ```bash
    $ tmux ls
    $ tmux list-sessions
    ```
    or, use `Ctrl + b s`

* Rename session

    `Ctrl + b $`

* Detach session

    `Ctrl + b d`

    Or use command mode

    ```bash
    : attach -d
    ```

* Attach session

    ```bash
    # last session
    $ tmux a
    $ tmux at
    $ tmux attach
    $ tmux attach-session

    # session with name
    $ tmux a -t mysession
    $ tmux at -t mysession
    $ tmux attach -t mysession
    $ tmux attach-session -t mysession
    ```
* Swith session

    `Ctrl + b (` (move to previous session)

    `Ctrl + b )` (move to next session)

* Kill session

    ```bash
    # kill by name
    $ tmux kill-ses -t mysession
    $ tmux kill-session -t mysession

    # kill all but the current
    $ tmux kill-session -a

    # kill all but mysession
    $ tmux kill-session -a -t mysession
    ```


## Windows

* Create window

    `Ctrl + b c`

* Switch window

    `Ctrl + b p` (previous)

    `Ctrl + b n` (next)

    `Ctrl + b 0 ... 9` (number)

* Rename window

    `Ctrl + b ,`

* Close window

    `Ctrl + b &`


## Panes

* Split panes

    `Ctrl + b %` (vertical)

    `Ctrl + b "` (horizontal)

* Show pane numbers
  
    `Ctrl + b q`

* Switch to panes

    `Ctrl + b` &uarr; &darr; &rarr; &larr; (directions)

    `Ctrl + b o` (next)

    `Ctrl + b q 0...9` (number)

* Resize panes

    `Ctrl + b + `&uarr; &darr; &rarr; &larr;

    `Ctrl + b` - `Ctrl + `&uarr; &darr; &rarr; &larr;

* Toggle Zoom

    `Ctrl + b z`

* Toggle layout
  
    `Ctrl + b Space`

* Move pane

    `Ctrl + b {` (left)
    
    `Ctrl + b }` (right)

* Convert to window

    `Ctrl + b !`

* Kill pane

    `Ctrl + b x`

## Others

* Enter command mode

    `Ctrl + b :`

* Set for all sessions

    ```bash
    : set -g OPTION
    ```

* Set for all windows

    ```bash
    : setw -g OPTION
    ```

* Help

    `Ctrl + b ?`
    ```bash
    $ tmux info
    ```

## Settings

`Ctrol + b :`
    ```bash
    $ set -g mouse on
    $ setw -g mouse on
    ```

Happy tmuxing!