stagescript
===========

A scripting language for non-programmers to create playable games.

Introduction
------------

There is a largely uncharted territory when it comes to writers expressing themselves through an interactive medium like computer games. When you write a book you have no way of writing multiple endings unless you write a Create-Your-Own-Adventure (CYOA) style book. Those books are all well and good and were the main inspiration for this project but they are a bit last century. CYOA books can also be very hard to visualize as a writer since you have to keep track of everything yourself. To make things worse, writers have no way of releasing their works as playable interactive projects without calling in a computer programmer, which are very hard to come buy and employing one throughout the creation process can be extremely expensive. Stagescript is aimed at solving this problem opening up the world of interactivity to writers and other non-programmers.

To clarify, Stagescript is nothing but text on a computer screen without its counterpart the Stagecraft Engine which allows the player to interact with the written story. Stagecraft Engine is a game engine available to both players and writers at http://fictiontheater.com.

Definitions
-----------

### Scenes

A scene is a definition for a playable entity within a collection of scenes that form a game.

*Example:*

```
Scene: Mountain top

A beautiful mountain top covered in snow ...
```

### Things

The word Thing followed by a unique name describes something that exists within the scene. It can be used to represent any tangiable object e.g. a character or an inanimate object like a book. The name of a thing is used as a reference in dialogs, events and rules. Unfortunately computers don't work very well with written text as it can easily become ambiguous you need to wrap references to things in [] like so [My Thing].

*Example:*

```
Thing: An old book

A rather large book bound in brown leather ...

Thing: Mysterious Woman

A dark figure sitting in the corner ...
```

As some non-programmers might find it weird to refer to characters and items as things. The syntax aliases *item*, *object* and *character* are provided to help ease non-programmers into the process of writing Stagescript. These aliases have no semantic relevance and items and characters continue to function exactly like things mentioned earlier.

*Example:*

```
Item: An old book

A rather large book bound in brown leather ...

Character: Mysterious Woman

A dark figure sitting in the corner ...

Object: Door

A whitely polished plastic door leading to the spaceship’s bridge ...
```

### Events

Games are very uninteressting without something to interact with. Events or commands as they are called as well define something that occurs as a result of some action most likely performed by the player. This can be choosing a specific conversation item, picking up an item or simply entering a scene. A list of events can be defined on the conversation scope or happening scope a character handing the player an item as a result of a conversation choice.

Example:
```
Item: Scumm Bar Token

This wooden token with a picture of a skull with crossed bones underneath will supposedly
get you into Scumm Bar. Let's at least hope so ... for now!

Character: Bearded guy

[Player] asks "Where can I go to become a mighty pirate?"
[Bearded guy] answers "You could try the Scumm Bar."
[Bearded guy] answers "Here you will need this to get in."
[Player] receives [Scumm Bar Token]
```

Not all events take place within conversations or the scope of characters and can be declared just as easily with event lines on a scene.

*Example:*

```
Scene: Mountain top

[Player] uses [Funny looking rock]
[Bearded guy] says “You’ll be sorry for that!...”
[Bearded guy] says “...or I can just find another rock to call mine. (sigh)”
[Player] receives [Funny looking rock]
```

Here we define directly on the scene what happens when the player examines an item. Below is a full listing of available happenings and their functionality within the game.

* **Dialog**
  * says - defines an utterance spoken by the source target
  * asks - defines a conversational request to a thing (see Conversations)
  * answers - defines a thing’s response to the preceding asks definition (see Conversations) ??should this be an alias??
* **Interaction**
  * uses - specifies that the source (player or another thing) can use to interact directly with a referenced thing
  * receives - tells the game to transfer the target thing to the target player or scene. When a receives command is run by the game engine the referenced thing is moved to the target.
* **Condition**
  * has - checks for the presence referenced thing in the player’s inventory (see Conditions)
  * missing - checks for the absence referenced thing in the player’s inventory
* **Switches**
  * becomes - is used to set the state of an item (see Switches)
* **Memories**
  * remembers - record an important consequence within the game for use later with the recalls action.
  * recalls - defines a condition (see Conditions) that is bound to something the player has done in the game that was recorded.
  * forgets - tells the source target to forget about the existing target consequence.
* **Travelling**
  * enters - defines the action when a player enters a scene
  * travels to - defines the transition to another scene defined by the reference (see Transitions)
* **Special**
  * narrates - a special form of dialog only available on the Game target. This is for expressing a narrative expression of what is happening (This narrative is displayed by default in the text version of Stagecraft but as optional captions in the graphical version).

All commands (happenings) can be stacked to form complex sequences of events which are demonstrated in the chapters below.

**TODO: define all these commands in more detail specifying allowed targets and destinations.**

### Conversations

A conversation is a set of event line definitions describing the interaction between the player and a referenced thing within a scene. There are utterances that are defined by specifying the source followed by the word says and intended dialog. The sequence of execution is implied by the top-down order in which the conversation lines are written. For clarity conversations can only reference things that have already been defined within a scene.

*Example:*

```
Character: Bearded guy

A bearded guy sitting on a stool by the bar. His eye-patch, skull decorated hat and
pegleg might suggest he’s a pirate. Maybe you should talk to him.

[Player] says "Hello! I’m Guybrush Threepwood and I want to be a pirate."
[Bearded guy] says "Yikes!"
[Bearded guy’s brother] says "Don’t sneak up on people like that."
[Player] says "I’m so sorry! It won’t happen again. Promise."

[Player] asks "What are you doing up here?"
[Bearded guy] answers "Oh just admiring the view."
[Bearded guy’s brother] answers "None of your damn business!"

[Player] asks "Where can I go to become a mighty pirate?"
[Bearded guy] answers You could try the Scumm Bar.
```

The above example will result in the first four lines being executed at the start of the dialogue and then presenting the player with the two choices defined below. Let’s go over it line by line for clarity.

```
Character: Bearded guy
```

This line starts the character’s scope within a scene and tells the game engine that everything that comes after is within the scope belonging to the character Bearded guy.

```
A bearded guy sitting on a stool by the bar. His eye-patch, skull decorated hat and
pegleg might suggest he’s a pirate. Maybe you should talk to him.
```

Every scene and thing has a description which the player sees upon entering a scene or examining an item. Stagescript is aimed at writers so the description part is strictly non-optional :).

[Player] says "Hello! I’m Guybrush Threepwood and I want to be a pirate."
[Bearded guy] says "Yikes!"
[Bearded guy’s brother] says "Don’t sneak up on people like that."
[Player] says "I’m so sorry! It won’t happen again. Promise!"

The first lines defines something the player says at the start of the conversation. The second and third lines define something that things within the scene would say next. Note that even though the the conversation is with Bearded guy another character Bearded guy’s brother can provide input. 

```
[Player] asks "What are you doing up here?"
[Bearded guy] answers "Oh just admiring the view."
[Bearded guy] answers "It is very beautiful up here."
[Bearded guy’s brother] answers "None of your damn business!"

[Player] asks "Where can I go to become a mighty pirate?"
[Bearded guy] answers "You could try the Scumm Bar. They have lots of shady pirates!"
```

The asks and answers lines are always defined together as they do not have semantic meaning on their own. The asks line defines a question the player can ask and is then followed by one or more answers lines specifying the response and the source that responds.

### Conditions

Conditions and switches are advanced declarations intended to control the flow of a scene and it’s elements. They can be used to hinder access to an exit, restrict conversation choices or control the visibility and state of characters and items.

*Example:*
```
[Player] has [Some item]
[Player] asks "What does this item do?"
[Character] says "How the hell should I know?"
[Player] says "You must know something about it. Right?"
[Character] says "It’s a unicorn summoner! There! I said it! Happy now?!"
```

The above example shows how the visibility of a conversation element can be controlled by providing a has restriction referencing a scene item. When using the word has the game checks to see if the player is in possession of the referenced thing. The negation of has is the missing restriction.

### Switches

Switches control the interaction and visibility state of an item. All items are by default Available and Visible.

* Available - indicates that an item is available to the player. An item marked as available can be used by the player.
* Unavailable - indicates that an item is unavailable to the player. An item marked as unavailable cannot be used by the player.
* Visible - indicates that an item is visible to the player. An item marked as visible can be seen by the player.
* Invisible - indicates that an item is not visible to the player. An item marked as invisible cannot be seen by the player and is therefore implicitly not available.

### Travels

A game would be rather boring with only one scene and and no way of specifying how the player can move between them. A travel is simply is the mechanism of moving from one scene to another and is defined on the player using the words travels to followed by a reference to a destination scene.

*Example:*

```
[Player] uses [Thing]
[Player] travels to [Destination Scene]
```

The corresponding event for the travel to event is the enters action. It is triggered whenever a player enters a scene after a transition. Just to state it explicitly; this event is also triggered on the opening scene in a game a.k.a. the first scene.

*Example:*

```
Scene: Destination Room

[Player] enters [Scene]
[Game] narrates “You enter the room”
```

### Memories

Memories is term used for events that the target (game, item, scene) “remembers“ about important decisions or actions the current player has made within the game for the purpose of making it easily available to other scenes or outside the game e.g. for use in a sequel or a series.

*Example:*
	
[Knight] says “Your mother was a hamster and your father smelt of elderberries”
[Game] remembers [Player was insulted by Knight]

Then at a later stage either in another scene or another game within a series the memories can be recalled by simply asking the game to recall a specified identifier.

*Example:*

```
[Player] asks "Hey, you look familiar"
[Local Drunk] recalls "Player was insulted by Knight"
[Local Drunk] says “I’m really sorry about calling your mother a hamster!”
[Local Drunk] says “...but your father he still smells of elderberries”
```

To close the circle on memories a memory can be forgotten as well.

*Example:*

```
[Crying Girl] recalls [Player was mean]
[Player] says “Whatever I did, I’m really sorry!”
[Crying Girl] forgets [Player was mean]
```

### Aliases

Because Stagescript is aimed first and foremost at writers careful consideration has been put into the words used for game commands. So, to make life easier and not to break the creative flow for as many writers as we possibly can we have created aliases for some commands and definitions. Punctuations in the form of ? and ! have also been added to emphasize the semantic difference between conditions and commands.

**TODO: defines aliases and punctuations (asks? forgets! recalls? says! remembers!)**

### Scopes

An advanced concept in Stagescript that needs clarifying is the concept of scopes. A scope is opened when something is defined with a header e.g. the start of a scene. Every definition that comes after the header is considered to be somehow related to the header. The only exception is something that defines another scope on the same level which causes the current scope to be closed and a new one to be opened. Scopes can have other scopes within them e.g. a scene might define a conversation which opens up a nested scope for the conversation within the scene.

Example:

```
Scene: Shady Bar            (new scene scope opened)
...
Character: Mysterious Man   (new character scope opened on scene scope)
...
Scene: Dark Alley           (new scene scope opened and previous closed)
```

All scopes allow a section of free text that is terminated by the next scope or by the first event line within the open scope.

*Example:*

```
Scene: Shady Bar           # (scene scope opened)

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce rhoncus libero eget nulla mattis,
nec aliquet est faucibus. Phasellus tincidunt diam non lectus ullamcorper ultricies. Phasellus
mi sapien, pretium at magna at, aliquet adipiscing mi.

Character: Mysterious Man	 # (character defined on scene)

Sed augue ligula, porta eleifend mattis eu, tempus sit amet dui. Curabitur pharetra at quam ut
ullamcorper. Curabitur eros ligula, ultricies non elit vitae, pulvinar commodo enim.

[Player] asks "So, how are you this fine morning?"
[Character] says "Go to hell! I'm trying to sleep"
```

The above example clearly demonstrates the use of free-text as a first element within a scope to provide a description for that scope. This is a writer’s language so the descriptional text at the beginning of a scope is obviously required.

### Comments

Writing a game in Stagescript is an iterative process and at some point you might become unsure how to complete some scene or if you're writing the game with somebody else you might want to say something without the player seeing it on their screen when playing. You might also want to make something you have written unplayable. This is where comments come in. You can put # anywhere you like and everything after that on the same line becomes a comment the Stagecraft Engine will ignore those lines completely so they are not part of the game.

```
# Check copyright issues before including this in the final release. 
#
# [Player] asks "Someone set up us the bomb?"
# [Evil Space Guy] answers "All your base are belong to us!"
```

Examples
--------

### Johnny’s Bedroom

This is a simple scene depicting a little boys bedroom and his need for a flashlight in order to explore the mysterious closet door in his room. The scene has two defined items on it a flashlight and a closet door. The flashlight is required to use the closet door which leads to the next scene called Dreamworld.

```
Scene: Johnny’s Bedroom

It’s the middle of the night and a storm is raging outside. You’re in your bedroom. The rain pounds
against the rooftop and the branches of the tree outside claw against the window glass making a
horrendous screeching noise. The light in your room is out and the thunderstorm has ensured power
outage in the entire neighbourhood. You can barely make out the silhouettes of the objects in your
room. Guess there is no use trying to fumble your way to the light switch, huh? Maybe you could try
finding your flashlight and hide in your closet until the storm passes.

Item: Flashlight

A standard pale green army issue flashlight your dad gave you on your 8th birthday last month. It
already has some scratches here but fully functional nonetheless. “Not to worry! These things were
built to last a lifetime.” Your father’s voice echoing in your head.

[Player] uses [Flashlight]
[Player] receives [Flashlight]

Object: Closet Door

A very common looking mahogany door with a worn brass handle leading to the built-in closet next
to your desk.

[Player] has [Flashlight]
[Player] uses [Closet Door]
[Player] travels to [Dreamworld]

[Player] missing [Flashlight]
[Player] uses [Closet Door]
[Player] says “I wouldn’t dare to enter the closet without some sort of light. I wonder where I put
my flashlight?”
```
