# Trigger Happy

`Trigger Happy` is an FVTT module which adds a powerful yet simple system for creating triggers and automating things within a world.

This was created as a way of creating the interactive FVTT Tutorial on [The Forge](https://forgevtt.com/game/demo)

# Installation

You can install this module by using the following manifest URL : `https://raw.githubusercontent.com/League-of-Foundry-Developers/fvtt-module-trigger-happy/master/module.json`

As GM go to the `Manage Modules` options menu in your World Settings tab then enable the `Trigger Happy` module.

# Video and Step by Step instructions

before we get into the details of how to use Trigger Happy, I think these links might be easier for people to get started quickly with using this module. Don't forget to come back and read the full README file to understand the full extent of how you can use Trigger Happy.

Here is a "Introduction to Trigger Happy" video tutorial by ['D&D Population Me'](https://patreon.com/dndpopulationme) : [https://www.youtube.com/watch?v=okTWYO4oEMA](https://www.youtube.com/watch?v=okTWYO4oEMA)

['D&D Population Me'](https://patreon.com/dndpopulationme) has also made two useful step by step guides for the most common use cases for using Trigger Happy :
- Token teleportation tutorial : [https://forums.forge-vtt.com/t/trigger-happy-teleportation-tutorial/2117/6](https://forums.forge-vtt.com/t/trigger-happy-teleportation-tutorial/2117/6)
- Scene Trap tutorial : [https://forums.forge-vtt.com/t/trigger-happy-traps/3448/4](https://forums.forge-vtt.com/t/trigger-happy-traps/3448/4)

Trigger Happy also comes with journal entries in its compendiums that gives useful examples of trigger scenarios you can use.

# How to use

To create triggers for a scene, it's very easy, create a journal entry with the title "Trigger Happy" (configurable in module settings) in which you will drag & drop actors, journals, macros, and anything else you want to trigger.

You can also create a Folder with the same "Trigger Happy" name and any journal entry inside that folder or in any of its own subfolders will be considered a trigger journal, so you can organize your trigger journals any way you want, when things start to get complex.

One line in the journal entry represents one trigger as long as the first link it contains is an Actor, a Token or a Scene. Any lines that do not have an actor or scene as their first link will be ignored, so feel free to organize your triggers however you see fit.

When a token that represents a trigger actor is clicked by a player, then any other links within that same line will be triggered. Macros will be executed, Scenes will be viewed, Roll tables will be rolled, Journal entries will be displayed and Tokens will be selected.

As an example, you can put this into your entry : 
```
# Traps in Dungeon Room 37
@Actor[Transparent actor above trap #1] @JournalEntry[You have triggered a trap]
@Actor[Transparent actor above trap #2] @JournalEntry[You have triggered a trap]

# Teleport the PC if they touch the magic circle, I warned them not to approach that room anyway, so it's their fault if they do
@Actor[Magical Circle] @Scene[Prison Cell] @JournalEntry[You are teleported] @RollTable[Magical side effect from teleportation] @Macro[Spawn token at position 100,200]

# Move the players to the next scene when they open the door to the basement
@Actor[Transparent token on door tile] @Scene[The basement]

# Display a dialog with the latest rumors the PCs hear when they enter the tavern, and show them the lcoal prices
@Scene[Tavern] @Macro[Display Dialog about rumors] @JournalEntry[Tavern room prices]

# The next 100 lines could create a complex sequence of triggers that automate the entire game so the DM's job is obsolete
```

The above example used the `@Actor[name]` format for simplicity, but when drag&dropping actors, they would appear in the journal entry as `@Actor[id]{name}`.

You can also use some non-official 'links' by using the same format : 
- `@Token[token name]` : If used as a trigger, this will cause the triggers to activate on any token with the specified name (you cannot set a token id in this case). As a trigger effect, it will cause the player to select the first token matching that name.
- `@ChatMessage[message contents]` : As an effect, this will send the specified message contents as a chat message
- `@Trigger[option1 option2 option3]` : This applies modifiers on the trigger line, keep reading for more information about available options.
- `@Drawing[label]` : This will trigger the effects when the player clicks/moves a token within the area of a drawing which has its text set to the `label` specified. Works best with rectangles.
- `@Door[coordinates]` : This will trigger the effects when a player opens or closes a door (based on options). The coordinates can be copy/pasted from the wall configuration sheet (excluding the `[` and `]`).

By using a `Token` trigger, you can have a single actor for your triggers (a door, a button or a transparent image) but setting a different and unique name for your tokens would allow you to use them as different triggers, without duplicating actors all over your actors directory.

By using the `ChatMessage` effect, you can send any message to chat. When used in combination with [The Furnace](https://github.com/kakaroto/fvtt-module-furnace)'s Advanced Macro system, you can use it to trigger macros with arguments specific to your trigger. 

You can also use the format `@ChatMessage[message contents]{speaker name alias}`

As an example : 

```
@Token[unique token name] @ChatMessage[Stop right there!]{Guard} @ChatMessage[/pan 1500 1500 0.3]

@Scene[World Map] @Token[Party Marker]
```

You can create and organize actors that would be used specifically for triggers, and drop them anywhere you want on the map. Using a transparent token has the best effect, and the players don't need to have any permissions for the token (or scene or journal to display) for the trigger to work.
The triggers on actors and tokens will work only if they click on the token in the case of visible tokens, and if the token is hidden (GM layer), then it will activate the trigger when the player moves their token within the trigger token. Note that they can always do a long move and jump over the token which would not trigger the effects.
Don't forget that you can also use token avatars as buttons, or change their width and height to fit your need.

If multiple trigger effects are in the same line, then they will be executed in sequence, waiting for the previous effect to finish before starting the next one.

## Advanced options

You can customize the behavior a little using the `@Trigger` pseudo link which allows you to set options.
The following options are available : 
- `ooc`: Will send any chat messages in that trigger as an out of character message
- `emote`: Will send any chat messages in that trigger as an emote
- `whisper`: Will send any chat messages in that trigger as a whisper to the GM
- `selfWhisper`: Will send any chat messages in that trigger as a whisper to the player who activates the trigger
- `preload`: Will cause any scene on the trigger line to be preloaded instead of switching the view to it
- `click`: Will cause the trigger token to activate on a click
- `move`: Will cause the trigger token to activate on a token move over it
- `stopMovement`: Will prevent any tokens from moving on top of the trigger token
- `capture`: Will cause the trigger token to capture any player moment that crosses it
- `doorClose`: Will cause a `@Door` trigger to trigger when the door is closed instead of the default when it opens.
- `doorOpen`: Will cause a `@Door` trigger to trigger when the door is open. This is the default, but it can be used along with `doorClose` option to have it trigger on both open and close

If a token is hidden (GM layer), then it is automatically considered a 'move' trigger, otherwise it's a 'click' trigger. You can override it with the `@Trigger[click]` or `@Trigger[move]` options, or you can specify both options to make a token trigger on both clicks and moves.

Contrarily to the `@Trigger[move]` triggers, which only activate when a token ends its movement on their, the `@Trigger[capture]` will trigger when a token crosses its path, which can be very useful to setting up a trap that the players cannot jump over. When a `capture` trigger is activated, the token movement will be stoped and the token will be moved to the center of the trigger. The token can only be moved out of the `capture` trigger if its starting position is the center of the trigger.

Here's an example of how these trigger options can be used together :

```
# When the player enters the scene, preload the next one
@Scene[Dungeon level 1] @Scene[Dungeon level 2] @Trigger[preload]

# When they click on the stairs, move them to the preloaded scene and select a token
@Token[Lvl 1 bottom stairs] @Scene[Dungeon level 2] @Token[a specific token they can control]

# Prevent the player from jumping into the bonfire like a moron
@Token[Bonfire] @ChatMessage[Your friends stop you from jumping into the fire, that can be dangerous]{GM} @Trigger[move stopMovement ooc]

# But let them touch it
@Token[Bonfire] @Trigger[click] @ChatMessage[You burn your hand slightly]{GM}

# If they click or try to move through the chasm, say something, but let them jump over it
@Token[hole in bridge over the chasm] @ChatMessage[The drop looks like 1000 feet]{Chasm} @Trigger[move click stopMovement ooc]
@Token[hole in bridge over the chasm] @ChatMessage[is scared] @Trigger[emote]


```


# Changelog
## 0.8.6
 - Added a config setting to disable/enable the trigger happy active/inactive button on the context menu.
 
## 0.8.5
- Foundry vtt 0.8 compatible.
- New config setting edge collision. If set tokens will be captured at the edge of a drawing/token rather than the center.
- fix the silly packaging error

## 0.8.3
- Foundry vtt 0.8 compatible.
- New config setting edge collision. If set tokens will be captured at the edge of a drawing/token rather than the center.

## v0.7

- Add support for labelled drawings as triggers

## v0.4.1

- Fix issue causing click triggers to fail for players not owning the trigger token

## v0.4

- Add support for capture triggers (@tposney)
- Add support for API changes in FVTT 0.5.4

## v0.3

- Add support for triggers when moving a token over a trigger token (@tposney)
- Fix a couple of bugs with regards to journal entries and chat messages (@tposney)
- Add the ability to trigger tokens by clicking on them even if they are hidden from the player
- Add support for `@Trigger[options]` links with options for move, click, stopMovement, ooc, emote, whisper, preload
- Add support for having multiple journals and journals within subfolders
- Fix new line detection when journal entry is written in preformatted text or div mode
- Add support for sending chat messages using an alias


## v0.2
- Add support for `@Actor[name]` links instead of only drag&dropped `@Actor[id]` links
- Add support for Token trigger
- Add support for sending chat messages as trigger effects (useful with advanced macros)
- Add support for setting a token as controlled as a trigger effect

## v0.1
- Initial release with support for Actor and Scene triggers

# License
This Foundry VTT module, writen by KaKaRoTo, is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).

This work is licensed under Foundry Virtual Tabletop [EULA - Limited License Agreement for module development v 0.1.6](http://foundryvtt.com/pages/license.html).
