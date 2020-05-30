# Getting started

## UI overview

![helio-ui]

This is what the sequencer page looks like in Helio, as of version 3. There are other pages besides this, but you'll spend most of the time in the sequencer.

The UI is separated into these parts:

* the **breadcrumb** [navigation control](#workspace-navigation) is on the top,
* the [**editor**](#editing-and-arranging) canvas is in the middle,
* the project **mini-map** is on the bottom,
* the track **navigation tools** are in the [left sidebar](#left-sidebar),
* the **editing tools** are in the [right sidebar](#right-sidebar).

They will be described below, but, before you dive in,

### A bit of a history and a couple of silly excuses

This project was started out of the need for an advanced MIDI editor — think something like Sublime Text for music.

I was also sick and tired of visual over-stimulation, which most of the music tools out there tend to have more and more (just google some pictures for "digital audio workstation"). As one of the main goals, I wanted a tool that feels right: something with an uncluttered and non-distractive UI.

So generally, I'm always trying to avoid adding UI controls if there's a way to do without them. As it turned out, though, there are a couple of challenges with that approach (for which I don't see simple solutions, UX design is hard):
 * one challenge is to keep the UI both simple or even minimalistic and not disorienting at the same time,
 * another challenge is to keep the UI look and behave consistent across all platforms, especially desktop and mobile

If something feels misleading to you — apologizes, feel free to [report](https://github.com/helio-fm/helio-workstation/issues/new) that to help identifying the main friction points.

### Workspace navigation

The breadcrumb control on the top is responsible for the navigation over the workspace.

The root node called `Studio` contains links to all available pages and open projects:

![breadcrumbs-root-menu]

#### Workspace structure

Breadcrumbs are displaying a hierarchy of the current page you're on.

Under the hood, all pages live in a tree-like structure like this:

- **Dashboard** for the projects list, a fancy logo, and a couple of buttons
- **Settings** for choosing a language, audio drivers, playing with UI flags, etc
- **Orchestra pit** for managing plugins and [instruments](#instruments) on the stage
  - **Instrument 1** page for setting up audio/MIDI routing for all plugins in your instrument
  - **Instrument 2** page and so on
- **Project 1** page for the basic project info: title, author, etc
  - [**Piano roll**](#piano-roll) for editing
  - [**Pattern roll**](#pattern-roll) for arranging
  - [**Version control**](#version-control) page
- **Project 2** page
  - [**Piano roll**](#piano-roll)
  - ..and so on

#### Context menus

Note that all items in the breadcrumb control have their own context menus:

![breadcrumbs-menus]


## Creating a Project

After starting Helio the first time you should have a simple example project already open in the editor. Here's a couple of steps that can help you to get started:

 * to rename the project, click `Example Project` in the breadcrumb control, and you'll see the project info page with some details available for editing,
 * after renaming, go back by pressing `Alt + Left Arrow` or by pressing the back button on the top left, and you should get back in the piano roll page,
 * note that you can only edit one track at a time: use right-click to switch to another track (or long tap on mobile),
 * switch to the pattern roll be pressing `Tab` or `Page Down`, or by clicking the uppermost button in the left sidebar, to play with arrangement,
 * double-click any clip to return to the piano roll with that clip in focus; at this point you should get an idea how things work in the sequencer layout.

To start a new project from scratch, navigate to the dashboard by pressing `Home` key, or clicking the `Studio` node in the breadcrumbs. There you'll see the list of recent projects, and a couple of buttons:

 * create an empty project,
 * open a project (this also imports MIDI files).

### Switching between projects

There are several ways:
 * use the `/` hotkey to show the projects list in the [command palette](tips-and-tricks.md#command-palette),
 * or hover the `Studio` item in the breadcrumb control, which shows the menu with all open projects (the most inconvinient way so far),
 * back and forward buttons also can be useful sometimes, the related hotkeys are `Alt + Cursor Left` and `Alt + Cursor Right`.


## Instruments

### Instruments management

The most notable difference in the instruments management from the most sequencers out there is that Helio separates instruments from projects.

Each project only holds the instrument "references" (basically, the hashcodes of the instrument info), so that the instrument settings are not saved in the project file, but rather in the application workspace settings.

Instruments are also created and set up in a separate page, called Orchestra Pit.

The reason for implementing it this way was ~~separation of concerns, yo~~ that in my workflow, I tend to use all the same instruments for all the projects. The app was designed primarily as a sketching and prototyping tool, and I usually have lots of sketches, so all the operations that involve switching between projects, opening and closing them, or checking out in the version control, were ought to be as fast as possible, and not eat up all the memory.

If your setup implies always having different instruments or instrument settings for each project, or if you want the project file to contain the instrument details, Helio will make you suffer.

On the other hand, if you have an instrument library you're comfortable with (e.g. VSL or some selected soundfonts collection), and you want to set it up once and forget about it, you'll probably like this approach.


### Orchestra pit page

The orchestra pit page has two sections:

 * all found plugins are displayed on the left side
 * all instruments on stage, created from those plugins, are on the right

![orchestra-pit]

First, you want to scan for available plugins, there are two options in the orchestra pit menu:

 * either pick a specific folder to scan (note that all subfolder will be scanned as well),
 * or perform a global plugins scan: it tries to guess the most possible locations, e.g. Steinberg folders in Program Files, and scans them and all their subfolders.

Once you have a list of plugins, create an instrument using the plugin menu, so that it appears on the stage and can be assigned to your tracks.

### Instrument details and routing

Double-click on any of the instruments to get to the instrument details page.

![instrument-routing]

Most of the actions here, including audio and MIDI routing, are available through the menus.

Interacting with nodes:

 * left-click on the node will create a plugin window, it it has one, or just select it, if it doesn't
 * right-click on the node will just select it

While it is possible to set up a multi-plugin instrument with audio/MIDI routing in Helio, the convenience of the instrument page was not of a particular concern: the development is rather focused on the sequencer functionality. If you are running it under Linux, it might be a good idea to add [Carla](https://kx.studio/Applications:Carla) as an instrument, and use it to manage VST/whatever plugins and routing.


## Editing and arranging

### Timeline

On the top of the editor canvas, there's a timeline control. To interact with it:

* left click at the timeline to position the playhead,
* middle-click to start the playback from that position,
* right click to invoke the timeline events menu:

![timeline-events]

Timeline events include annotations, key signatures and time signatures, and they don't affect the playback of your piece in any way, they are rather meant to provide a visual cue.

Manipulating the timeline events:

* click on any event to focus the playhead at its position,
* once focused, click again to edit or delete the event (displays a dialog),
* drag to move, or shift-drag to duplicate the event.

#### Annotations

Annotations are just text markers with optionally adjustable length:

![timeline-annotations]

The right-click on the annotation selects all notes of the active track up to the next annotation.

#### Time signatures

Time signatures simply define the way the vertical grid lines are aligned in the roll:

![timeline-time-signatures]

The right-click on the time signature selects all notes of the active track up to the next time signature.

#### Key signatures

Key signatures change the way the rows are highlighted in the piano roll, but this effect can be [disabled](tips-and-tricks.md##ui-flags).

![timeline-key-signatures]

Apart from that, arpeggiators and a [couple](tips-and-tricks.md#chord-tool) of [other tools](tips-and-tricks.md#quick-rescale-tool) rely on the key signatures to figure out the current harmonic context.

#### Reprise

If you have enabled the playback loop over the selected scope, timeline will display the repeat signs:

![timeline-reprise]

### Left sidebar

This sidebar is responsible for track navigation and UI control.

Most buttons on the sidebars have keyboard shortcuts, which makes then kinda redundant, but they are displayed anyway for the sake of having a consistent UI on mobiles or touch-screen laptops, where you don't have hotkeys.

 * ![sidebar-left-1] — switch the editor view between the piano roll and the pattern roll (`tab`),
 * ![sidebar-left-2] — zoom out (`shift + z`), zoom in (`z`), and zoom selection (`control + tab`),
 * ![sidebar-left-3] — jump over the timeline events (`,` and `.`),
 * ![sidebar-left-4] — toggle the [velocity map](#velocity-map) (`v`),
 * ![sidebar-left-5] — UI [flags](tips-and-tricks.md##ui-flags) that toggle scales highlighting and the note guides (`h` and `g`),
 * ![sidebar-left-6] — a simple waveform or spectrogram view.

### Right sidebar

This sidebar is responsible for editing tools and playback control:

 * ![sidebar-right-1] — toggle the playback loop over the selection (`f11`),
 * ![sidebar-right-2] — edit [modes](#edit-modes) (`1`, `2`, `3`, `4`, `5`),
 * ![sidebar-right-3] — some other tools - the chord tool and arpeggiators, if available,
 * ![sidebar-right-4] — copy and paste, undo and redo,
 * ![sidebar-right-5] — playback (`space` or `enter`) and recording (`f12`) control.

## Piano roll

... todo the concept of active track and clip

### Edit modes

... todo describe edit modes
... todo also tell about drag-to-copy, group resize, undo (note that the last 10 undo actions are saved in the project and are available after restarting the app), etc

### Velocity map

The velocity levels editor (toggled by `v` hotkey) provides a way to visualize and draw gradual increase/decrease in note volume.

As well as the piano roll, the velocity map limits its editable scope to the active track. But in addition, if any notes are selected, the editable scope is limited to the selection, to make it easier to draw more complex ramps for different chunks of the track:

![velocity-map-toggle]

At the moment of writing, only linear ramps are implemented:

![velocity-map-ramps]

## Pattern roll

You don't necessarily need that editor. Helio was designed to be a hybrid linear-based/pattern-based sequencer, so you could just stay in the piano roll mode and treat your project as one big canvas.

However, the pattern roll is helpful for rearranging experiments:

![patterns]

Pattern roll also allows to tweak some track parameters, like key offset of velocity multiplier. In future, it may shift towards more parametric sequencer features.

## MIDI recording

The record button will try to auto-detect the available and enabled MIDI input device, or provide a choice if there are more than one:

![...]

If the recording mode is on, but the playback has not started yet, it will wait until it receives the first MIDI event and start recording & playback.

In the piano roll mode, it always records to the selected track/clip:

In the pattern roll, it either also records to the selected track/clip, or, if no piano clip is selected, it adds one, once the actual recording starts:

![...]


## Version control

### Basic concepts

...

### Saving and resetting changes

...

### Synchronization across devices

...




[helio-ui]: images/screen-v3.png "UI overview"
[orchestra-pit]: images/orchestra-pit.png "The instruments management page"
[instrument-routing]: images/instrument-routing.png "The instrument details page"

[breadcrumbs-root-menu]: images/breadcrumbs-root-menu.png "Breadcrumbs control, root menu"
[breadcrumbs-menus]: images/breadcrumbs-menus.png "Breadcrumbs control, conetxt menus"

[patterns]: images/patterns-arrange.png "Pattern mode for arrangements"
[patterns-clips]: images/patterns-track-clips.png "Track instances (clips) and their modifications"

[velocity-map-toggle]: images/velocity-map-toggle.png "Velocity map overview"
[velocity-map-ramps]: images/velocity-map-ramps.png "Velocity map linear ramps"

[timeline-events]: images/timeline-events.png "Timeline: interacting and events menu"
[timeline-annotations]: images/timeline-annotations.png "Timeline: annotation events"
[timeline-key-signatures]: images/timeline-key-signatures.png "Timeline: key signature events"
[timeline-time-signatures]: images/timeline-time-signatures.png "Timeline: time signature events"
[timeline-reprise]: images/timeline-reprise.png "Timeline: repeat signs"

[sidebar-left-1]: images/sidebar-left-1.png "Navigation sidebar 1"
[sidebar-left-2]: images/sidebar-left-2.png "Navigation sidebar 2"
[sidebar-left-3]: images/sidebar-left-3.png "Navigation sidebar 3"
[sidebar-left-4]: images/sidebar-left-4.png "Navigation sidebar 4"
[sidebar-left-5]: images/sidebar-left-5.png "Navigation sidebar 5"
[sidebar-left-6]: images/sidebar-left-6.png "Navigation sidebar 6"

[sidebar-right-1]: images/sidebar-right-1.png "Tooling sidebar 1"
[sidebar-right-2]: images/sidebar-right-2.png "Tooling sidebar 2"
[sidebar-right-3]: images/sidebar-right-3.png "Tooling sidebar 3"
[sidebar-right-4]: images/sidebar-right-4.png "Tooling sidebar 4"
[sidebar-right-5]: images/sidebar-right-5.png "Tooling sidebar 5"