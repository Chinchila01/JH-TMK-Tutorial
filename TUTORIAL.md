# Writing custom keyboard firmware using Jack Humbert’s fork of TMK
## 1. Introduction
Welcome! The purpose of this tutorial is to take someone knowing nothing about writing custom firmware for a keyboard and give them the tools to write one for *Jack Humbert’s fork of tmk_keyboard.* Note that last part carefully, what is written here is written for [Jack Humber's fork of TMK](https://github.com/jackhumbert/tmk_keyboard), and will **only** work with this version of tmk_keyboard.

This tutorial is split into two sections: ‘Basics’ and ‘Beyond the Basics.’ By following the ‘Basics’ section you will be taken from a blank file to the uploading of your firmware. This might very well be all you need. The ‘Beyond the Basics’ section deals with more advanced topics.

### 1.1 Method
The ‘Basics’ section of this tutorial will be done in the style of following the implementation of *a particular* keymap from start to finish. This is a keymap for a Planck ortho—linear keyboard. This has three advantages:
* Programmable keyboards offer, by definition, an enormous array of possible configurations and uses. The task of outlining all of them would be impossible. Nonetheless the principles involved in any one implementation are *transferrable* to any other. By following *one* implementation you will gain the skills to write virtually any customisation.
* You will see the principle of iterative refinement at work, which is a part of any project of this kind.
* By following the tutorial and at each step copying what is done modified for your own requirements you can come out the other end with a finished product that is very close to—if not exactly—what you want. It is therefore recommended that you **follow and adapt** your requirements as they go along.

There is one main disadvantage:
* You will not see all functions in use.

You will see a great many functions, however, and many I don’t use will be explained where appropriate. The skills you pick up should enable you to use these without difficulty. 

### 1.2 Layout
Each section is numbered so that I can refer you forward or backwards easily. I begin each major section with a summary of what that section covers. This accomplishes three things.
* It gives you an idea of what is about to happen. The human brain processes information better if it has a structure to hang the new material in.
* You can read this and see whether you need the information that section gives.
* It also serves as a reminder should you need to refresh the contents of that section.

In doing this is am shamelessly copying ancient and medieval authors whose tables of contents and section introductions are a standard to aspire to (if you’re interested look at Augustine’s *On the Trinity*, as a good example).

### 1.3 Target Audience
Who is this tutorial for? Well, you are programming custom firmware for a mechanical keyboard so I assume that you have basic technical skills. To make it through the ‘Basics’ section—all many are likely to need—you should be able to edit text files, and run commands in a terminal. That’s it. After that there are one or two things in the ‘Beyond the Basics’ section which require some c.

### 1.4 Some Technecalia
I have elected to use the extended rather than standard keymap. The standard keymap was extended to allow for many more function definitions and some other nice tricks. There are very few reasons not to use the extended keymap. I will not discuss the standard keymap in the tutorial.

It's okay if this does not make sense to you. But you might need the information further into your journey, so I have given it here.

### 1.5 Disclaimer
This information is provided as a guide and every effort has been made to make sure it is correct, but it will not be perfect. Use it at your own risk.

## 2. The Basics
### 2.1 Installing the Right Tools

**Jack, I was thinking about cannibalising your existing [Tutorial](https://github.com/jackhumbert/tmk_keyboard/blob/master/doc/build.md) for this part. I'll post credit at the end naturally, but are you okay with that?**

#### 2.1.1 Summary
In this section you will download the appropriate version of the tmk_keyboard firmware as well as dfu_programmer and operating system specific development tools so that you have everything you need compile and transfer (flash) the firmware. You will then verify the installation.

#### 2.1.2 Downloading the Software and setting up the environment. 

##### 2.1.2.1 Windows
1. Install [WinAVR Tools](http://sourceforge.net/projects/winavr/) for AVR GCC compiler.
2. Install [DFU-Programmer][dfu-prog] (the -win one).
3. Start DFU bootloader on the chip first time you will see 'Found New Hardware Wizard' to install driver. If you install device driver properly you can find chip name like 'ATmega32U4' under 'LibUSB-Win32 Devices' tree on 'Device Manager'. If not you will need to update its driver on 'Device Manager' to the `dfu-programmer` driver.

##### 2.1.2.2 Mac
1. Install [CrossPack](http://www.obdev.at/products/crosspack/index.html) or install Xcode from the App Store and install the Command Line Tools from `Xcode->Preferences->Downloads`.
2. Install [DFU-Programmer][dfu-prog].

##### 2.1.2.3 Linux
1. Install AVR GCC with your favorite package manager.
2. Install [DFU-Programmer][dfu-prog].

#### 2.1.3 Verify Your Installation
1. Clone the following repository: https://github.com/jackhumbert/tmk_keyboard
2. Open a Terminal and `cd` into `tmk_keyboard/keyboard/planck`
3. Run `make`. This should output a lot of information about the build process.

### 2.2. Keymaps
#### 2.2.1 Summary
In this section you will create a new keymap file with the name `extended_keymap_<name>` in the path `tmk_keyboard/keyboard/planck/extended_keymaps/`, where <name> is a value of your choice. You will then add the `#include "extended_keymap_common.h"` statement to the beginning, and write the default layer of the keyboard within a `const uint16_t PROGMEM keymaps[][MATRIX_ROWS][MATRIX_COLS] = { };` statement as a series of rows delimited between braces and separated by commas (except for the last row) and a series of columns delimited by commas within those braces. You will then learn that keys are assigned via a KC_<KEY> and that additional layers can be created by repeating the process. 

#### 2.2.2 Creating a keymap file

Using your favourite text editor create a new file with the name `extended_keymap_<name>` in the folder `tmk_keyboard/keyboard/planck/extended_keymaps/`, where <name> is a value of your choice. In other words, if you are in the tmk_keyboard directory go to the folder `keyboard/planck/extended_keymaps` or `keyboard/quark/extended_keymaps` and create your file.

This tutorial will be creating a file for a plank keyboard but, again, the process is similar for a quark.

#### 2.2.3 Writing Your First Keymap

The first line of your file should be `#include "extended_keymap_common.h"`. Don't worry about what it does, just put it at the top of your file and add a space or two afterwards.

After doing that we are now going to create your first keymap. Copy and paste the following code into your editor:

```
/* MIT Layout (default)
 *
 * ,-----------------------------------------------------------------------.
 * |     |     |     |     |     |     |     |     |     |     |     |     |
 * |-----------------------------------------------------------------------|
 * |     |     |     |     |     |     |     |     |     |     |     |     |
 * |-----------------------------------------------------------------------|
 * |     |     |     |     |     |     |     |     |     |     |     |     |
 * |-----------------------------------------------------------------------|
 * |     |     |     |     |     |           |     |     |     |     |     |
 * `-----------------------------------------------------------------------'
 */
  [0] = {
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     }, 
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     },
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     },
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     }
  },
```

At the top is a section which is commented out, this means that the software that converts this file into a form ready to send to the keyboard will ignore it. It is there so we can write in a keymap using just the alphanumeric symbols we are used to, rather than keycodes. It is human rather than machine legible.

##### 2.2.3.1 The Alphabet

Let's take the first step in writing our keymap by adding the alphabet characters. I am a qwerty typist, so our aim to have the following:

```
/* MIT Layout (default)
 *
 * ,-----------------------------------------------------------------------.
 * |     |  q  |  w  |  e  |  r  |  t  |  y  |  u  |  i  |  o  |  p  |     |
 * |-----------------------------------------------------------------------|
 * |     |  a  |  s  |  d  |  f  |  g  |  h  |  j  |  k  |  l  |     |     |
 * |-----------------------------------------------------------------------|
 * |     |  z  |  x  |  c  |  v  |  b  |  n  |  m  |     |     |     |     |
 * |-----------------------------------------------------------------------|
 * |     |     |     |     |     |    spc    |     |     |     |     |     |
 * `-----------------------------------------------------------------------'
 */
 ```

Okay, we've got our alphabet keymap conceptualised the way we want. Let's fill in the part that actually gets compiled.

```
  [0] = {
    {    , KC_Q, KC_W, KC_E, KC_R, KC_T, KC_Y, KC_U, KC_I, KC_O, KC_P,    }, 
    {    , KC_A, KC_S, KC_D, KC_F, KC_G, KC_H, KC_J, KC_K, KC_L,     ,    },
    {    , KC_Z, KC_X, KC_C, KC_V, KC_B, KC_N, KC_M,     ,     ,     ,    },
    {    ,     ,     ,     ,     ,     KC_SPC,     ,     ,     ,     ,    }
  },
```
Each key is prefixed with `KC_` followed by the key name. The alphabet keys are easy, as they are simply the capitalised version of each letter.

You might not like the above layout. Feel free to swap things around.

##### 2.2.3.2 MIT and Grid Layout 

You will notice that inside the curly braces after `const uint16_t PROGMEM keymaps[][MATRIX_ROWS][MATRIX_COLS] = { };` there are a series of rows delimited between braces and separated by commas (except for the last row). Inside the curly braces that make up each row are a series of columns delimited by commas within those braces. You adjust these parameters according to the lay of the keyboard. I have shown a keymap for an MIT layout. But what about a grid? Simple. You just add another comma to the bottom row. So we get:

```
  [0] = {
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     }, 
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     },
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     },
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     }
  },
```
##### 2.2.3.3 The Full Keyset

Okay, let's add the modifier keys and punctuation. We'll follow the default layout to begin with.

```
/* MIT Layout (default)
 *
 * ,-----------------------------------------------------------------------.
 * | tab |  q  |  w  |  e  |  r  |  t  |  y  |  u  |  i  |  o  |  p  | bspc|
 * |-----------------------------------------------------------------------|
 * | esc |  a  |  s  |  d  |  f  |  g  |  h  |  j  |  k  |  l  |  ;  |  '  |
 * |-----------------------------------------------------------------------|
 * |shift|  z  |  x  |  c  |  v  |  b  |  n  |  m  |  ,  |  .  |  /  |enter|
 * |-----------------------------------------------------------------------|
 * |     | ctl | alt | meta|     |    spc    |     | left| down| up  |right|
 * `-----------------------------------------------------------------------'
 */

  [0] = {
    {KC_TAB,  KC_Q,    KC_W,    KC_E,    KC_R, KC_T, KC_Y, KC_U, KC_I,    KC_O,    KC_P,    KC_BSPC}, 
    {KC_ESC,  KC_A,    KC_S,    KC_D,    KC_F, KC_G, KC_H, KC_J, KC_K,    KC_L,    KC_SCLN, KC_QUOT},
    {KC_LSFT, KC_Z,    KC_X,    KC_C,    KC_V, KC_B, KC_N, KC_M, KC_COMM, KC_DOT,  KC_SLSH, KC_ENT},
    {       , KC_LCTL, KC_LALT, KC_LGUI,     ,   KC_SPC,       , KC_LEFT, KC_DOWN, KC_UP, KC_RGHT}
  },
```
Don't worry about the blank spaces at the moment. Hopefully you have a good idea of how to assign the basic keys now. You might want to change the above layout, or add keys that are not present. For a full list of keycodes see `tmk_keyboard/doc/keycode.txt`.

#### 2.2.4 Layers

Obviously the point of a customisable keyboard is that we can do much more than this. So lets add some more layers. Think of layers as operating like the shift key on a normal keyboard. When you press shift a range of new characters (capitals and other punctuation marks) are available on the same keys which would ordinarily produce other characters. So let's create our first layer.

Get another copy of the blank keymap above and paste it below your existing keymap, but still with the `};` environment. You will end up with something like this:
```
const uint16_t PROGMEM keymaps[][MATRIX_ROWS][MATRIX_COLS] = {
/* MIT Layout (default)
 *
 * ,-----------------------------------------------------------------------.
 * | tab |  q  |  w  |  e  |  r  |  t  |  y  |  u  |  i  |  o  |  p  | bspc|
 * |-----------------------------------------------------------------------|
 * | esc |  a  |  s  |  d  |  f  |  g  |  h  |  j  |  k  |  l  |  ;  |  '  |
 * |-----------------------------------------------------------------------|
 * |shift|  z  |  x  |  c  |  v  |  b  |  n  |  m  |  ,  |  .  |  /  |enter|
 * |-----------------------------------------------------------------------|
 * |     | ctl | alt | meta|     |    spc    |     | left| down| up  |right|
 * `-----------------------------------------------------------------------'
 */

  [0] = {
    {KC_TAB,  KC_Q,    KC_W,    KC_E,    KC_R, KC_T, KC_Y, KC_U, KC_I,    KC_O,    KC_P,    KC_BSPC}, 
    {KC_ESC,  KC_A,    KC_S,    KC_D,    KC_F, KC_G, KC_H, KC_J, KC_K,    KC_L,    KC_SCLN, KC_QUOT},
    {KC_LSFT, KC_Z,    KC_X,    KC_C,    KC_V, KC_B, KC_N, KC_M, KC_COMM, KC_DOT,  KC_SLSH, KC_ENT},
    {       , KC_LCTL, KC_LALT, KC_LGUI,     ,   KC_SPC,       , KC_LEFT, KC_DOWN, KC_UP, KC_RGHT}
  },
/* MIT Layout (default)
 *
 * ,-----------------------------------------------------------------------.
 * |     |     |     |     |     |     |     |     |     |     |     |     |
 * |-----------------------------------------------------------------------|
 * |     |     |     |     |     |     |     |     |     |     |     |     |
 * |-----------------------------------------------------------------------|
 * |     |     |     |     |     |     |     |     |     |     |     |     |
 * |-----------------------------------------------------------------------|
 * |     |     |     |     |     |           |     |     |     |     |     |
 * `-----------------------------------------------------------------------'
 */
  [1] = {
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     }, 
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     },
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     },
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     }
  }
};
```

Notice two very important things. A comma has been added to the end of the first layer `},` and the number in the square brackets at the beginning of the second layer is an increment by one of the previous layer, giving us `[1]` in this case. Remember the comma. When you're dealing with code these small details mean the difference between a successful compilation and not. The number in square brackets is how we are able to refer the layer in the code that we will be writing to select them. The first layer is layer 0, the next layer is layer 1.

This is how I have layer 1 configured, notice that I have included media keys. The first two keys on the third row are for brightness control on a Mac, which are actually the pause and scroll lock keys. **Important:** as I use a Mac I have used `KC_MRWD` and `KC_MFFW` to move fowards or backwards a track. In order to do this on a non-mac use `KC_MNXT` and `KC_MPRV` 

```
 /* MIT Layout (Raised)
 *
 * ,-----------------------------------------------------------------------.
 * |     | f1  | f2  | f3  | f4  | f5  | f6  | f7  | f8  | f9  | f10 | f11 |
 * |-----------------------------------------------------------------------|
 * |     |  1  |  2  |  3  |  4  |  5  |  6  |  7  |  8  |  9  |  0  | f12 |
 * |-----------------------------------------------------------------------|
 * |     | br+ | br- | mute| vol-| vol+| bwd | ply | fwd |  [  |  ]  |  =  |
 * |-----------------------------------------------------------------------|
 * |     |     |     |     |     |           |     |     |     |     |     |
 * `-----------------------------------------------------------------------'
 */
  [1] = {
    {KC_TRNS, KC_F1,   KC_F2,   KC_F3,   KC_F4,   KC_F5,   KC_F6,   KC_F7,   KC_F8,   KC_F9,  KC_F10,   KC_F11 },
    {KC_TRNS, KC_1,    KC_2,    KC_3,    KC_4,    KC_5,    KC_6,    KC_7,    KC_8,    KC_9,    KC_0,    KC_F12 },
    {KC_TRNS, KC_PAUS, KC_SLCK, KC_MUTE, KC_VOLD, KC_VOLU, KC_MRWD, KC_MPLY, KC_MFFW, KC_LBRC, KC_RBRC, KC_EQL },
    {KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS,     KC_TRNS,      KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS}
  }
```

Notice the keycode `KC_TRNS`. This is short for transparent. What this means is that when a key with `KC_TRNS` is pressed the software looks each layer below until there is a keycode other than `KC_TRNS` and sends that keycode. In other words, with our current configuration the space bar will be treated as space bar in both layers, as in layer 1 the key is transparent and falls back to layer 0. The upshot of this is that we have to be careful with how we assign keys. If we decide to make a key toggle layers while held we can't overwrite that key in the new layer or it will not return to the original layer when released. We need to keep toggle keys transparent in the layers they switch to.

Speaking of switching layers, we now need to know how to do just that.

### 2.3. Functions
#### 2.3.1 Summary
In this section you will learn that predefined functions that enable a range effects—from basic layer shifting to complex behaviour such as the assignment of dual roles to keys—are placed inside a `const uint16_t PROGMEM fn_actions[] = { };` environment and assigned to the keymap as `F(<function number>)`.
#### 2.3.2 Activating Layers
So how do we activate our new layer? We need to use one of the predefined functions, or 'actions.' Add the following beneath everything else currently in your file:

```
const uint16_t PROGMEM fn_actions[] = {
[1] = ACTION_LAYER_MOMENTARY(1)
};
```

This action makes layer 1 active on the key press event and inactive on the release event. In other words the layer is active as long as the you hold the key down.

The syntax of the function works as follows. The layer to be switched to is the one in the round brackets after the function name. We want layer 1 so we have `(1)`.

The number in the square brackets at the start of the line is the number of the function itself. Since it is the first in our list we have `[1]`. In order to call the action we have written here we use the this number inside of a F(<number here>) keycode, here F(1). Thus, adding it to our layer 0 we have:

```
  [0] = {
    {KC_TAB,  KC_Q,    KC_W,    KC_E,    KC_R, KC_T, KC_Y, KC_U, KC_I,    KC_O,    KC_P,    KC_BSPC}, 
    {KC_ESC,  KC_A,    KC_S,    KC_D,    KC_F, KC_G, KC_H, KC_J, KC_K,    KC_L,    KC_SCLN, KC_QUOT},
    {KC_LSFT, KC_Z,    KC_X,    KC_C,    KC_V, KC_B, KC_N, KC_M, KC_COMM, KC_DOT,  KC_SLSH, KC_ENT},
    {       , KC_LCTL, KC_LALT, KC_LGUI,     ,   KC_SPC,  F(1) , KC_LEFT, KC_DOWN, KC_UP, KC_RGHT}
  },
```
The key to the right of space now switches to layer 1.
#### 2.3.3 Beyond Basic Activation

There are some very clever things that can be done with layer actions as well as modifier actions. Let's look at three examples.

##### 2.3.3.1 Example 1

Consider our layer 1. It is good, but the keys we have there have yet further symbols accessed via shift. When shift is held `9` becomes `(`. Having to hold two keys down—the layer key plus shift—is inconvenient. A layer action comes to the rescue. `ACTION_LAYER_MODS(<LAYER>, <MODIFIER>)` activates a layer and holds a modifier key down for us at the same time.

So if we add `[2] = ACTION_LAYER_MODS(1, MOD_LSFT),` after our first function we solve the problem. The shift key is now held down as we access the layer. Just add F(2) to your keymap. Note, however, that shift is not `KC_LSFT` here but `MOD_LSFT`. When supplying a modifier key to a function that expects a modifier as an argument we need to use this latter form.

##### 2.3.3.2 Example 2

I like having modifier keys such as Control and Meta within easy reach, but also rely heavily on keys such as escape. Easily accommodated, use `ACTION_MODS_TAP_KEY(<MODIFIER>, <KEY>)`. This function allows you assign a key the behaviour of being a modifier key when held down, but sending a different keycode when tapped. If you want to have Control act as Escape when tapped (very useful for vim users) just add `[3] = ACTION_MODS_TAP_KEY(MOD_LCTL, KC_ESC),` and put F(3) in your keymap at the appropriate place.

##### 2.3.3.2 Example 3

I use a typesetting environment called LaTeX much of my day. I write for this environment in emacs. Emacs commands rely on often cumberson combinations of keystrokes. I want to have a layer dedicated to such commands, but how to switch to that layer? I don't want that process itself to be cumberson. Enter `ACTION_LAYER_TAP_KEY(<LAYER>, <KEYCODE>)`. This allows a key to send a normal (non-modifier) key when tapped, but to switch layers when held.

I can bind this to `KC_L` for latex and place relevant commands in places where I will easily remember them. The function would look like this: `[8] = ACTION_LAYER_TAP_KEY(4, KC_L),` where layer 4 is where I have set up my latex keys.

#### 2.3.4 A List of Actions

You should now have a good feel for how layer actions work. The following list shows the action layer options that you are most likely to find useful.

To set the default layer and activate it use:

`ACTION_DEFAULT_LAYER_SET(layer)`

To activate a layer when key a key is pressed and deactivate when released use:

`ACTION_LAYER_MOMENTARY(layer)`

To turn on a layer on first tap, and then leave the layer on the second tap use:

`ACTION_LAYER_TOGGLE(layer)`

Register a selected keystroke on tap, but turn on a layer while held: 

`ACTION_LAYER_TAP_KEY(layer, key)`

This action allows you to either turn on a layer by holding the key, or to toggle it after a series of taps. Number of taps it takes to toggle the layer can be configured by changing the variable TAPPING_TOGGLE in config.h. It is 5 by default.

`ACTION_LAYER_TAP_TOGGLE(layer)`

To switch to a layer while simulating that certain modifier keys are held down while the layer is active use:

`ACTION_LAYER_MODS(<LAYER>, <MODIFIERS>)`

**IMPORTANT:** See section 2.3.5 for information on how you can use multiple modifer keys at the same time here.

#### 2.3.5 Modifier Functions

Another section of actions that you can perform involves modifier keys. Say that you want to have a key that simulates Control-Alt-Delete, or some other keychord. You can use the function `ACTION_MODS_KEY(<MODIFIER>, <KEY>)` for this. To simulate multiple modifiers being held, use a pipe `|`:

```
ACTION_MODS_KEY(MOD_LCTL | MOD_LALT, KC_DELETE)
```

If instead you just want to use a key to simuate multiple modifers being held down (say a particular program you use a lot requires you hold down Control and Alt a great deal) you can use `ACTION_MODS(<MODIFIERS>)`:

```
ACTION_MODS(MOD_LCTL | MOD_LALT)
```

If you want to have a modifer apply to the next key you press without having to hold it down (i.e. If you want to use shift to capitalise a letter but tapping shift, then tapping the letter, use:

```
ACTION_MODS_ONESHOT(<MODIFIER>)
```

##### 2.3.5.1 Example Modifier Usage

I have a layer set up on my keyboard which activates when I hold down `a` which allows me to use the arrow keys without leaving my home row (for those interested I have them bound vim-style to hjkl). But on a Mac there are built-in commands for moving the cursor by word or by line (Alt-left/right and Control-a/e respectively). I would like to access these in a single keystroke both for convinence and because my left hand in already holding down a key. I therefore have a keymap set up like this:

```
 /* MIT Layout (Arrowkeys).
 *  I have put the actual output in the outline.
 * ,-----------------------------------------------------------------------.
 * |     |     |     |     |     |     | SOL |WDLFT|WDRGT| EOL |     |     |
 * |-----------------------------------------------------------------------|
 * |     |     |     |     |     |     | LEFT| DOWN| UP  |RIGHT|     |     |
 * |-----------------------------------------------------------------------|
 * |     |     |     |     |     |     |     |     |     |     |     |     |
 * |-----------------------------------------------------------------------|
 * |     |     |     |     |     |           |     |     |     |     |     |
 * `-----------------------------------------------------------------------'
 */
  [3] = {
    {KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, F(9),    F(11),   F(12),   F(10),   KC_TRNS, KC_TRNS},
    {KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_LEFT, KC_DOWN, KC_UP,   KC_RGHT, KC_TRNS, KC_TRNS},
    {KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS},
    {KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS,     KC_TRNS,      KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS, KC_TRNS}
```

And supporting functions like this:

```
const uint16_t PROGMEM fn_actions[] = {
...
  [9] = ACTION_MODS_KEY(MOD_LCTL, KC_A), //Navigate text outside of evil in osx
  [10] = ACTION_MODS_KEY(MOD_LCTL, KC_E), //Navigate text outside of evil in osx
  [11] = ACTION_MODS_KEY(MOD_LALT, KC_LEFT), //Navigate text outside of evil in osx
  [12] = ACTION_MODS_KEY(MOD_LALT, KC_RGHT), //Navigate text outside of evil in osx
};
```
### 2.4. Macros
#### 2.4.1 Summary
In this section you will learn that a macro is used for simulating for complex keystrokes and is placed in a `const macro_t *action_get_macro(keyrecord_t *record, uint8_t id, uint8_t opt) { };` environment using a `case` structure.

#### 2.4.2 Defining Macros
Say you want to have complex set of keystrokes like 'Control-c, Control-f, Control-e' happen when you press a key. Or perhaps you want a certain word to be written. Macros can do this for you.

### 2.5. Compilation and Flashing

#### 2.5.1 Compilation Functions

Here is a list of some of the functions available from the command line. We will be going through the most common usage scenario, but this will serve as a handy reference for the future.

* `make clean`: clean the environment - may be required in-between builds
* `make`: compile the code
* `make COMMON=true`: compile with the common (non-extended) keymap
* `make MATRIX=<matrix_file>`: compile with the referenced matrix file. Default if unspecified is `matrix_pcb.c`. For handwired boards, use `matrix_handwired.c`.
* `make KEYMAP=<keymap>`: compile with the extended keymap file `extended_keymaps/extended_keymap_<keymap>.c`
* `make COMMON=true KEYMAP=<keymap>`: compile with the common keymap file `common_keymaps/keymap_<keymap>.c`
* `make dfu`: build and flash the layout to the PCB
* `make dfu-force`: build and force-flash the layout to the PCB (may be require for first flash)

Generally, the instructions to flash the PCB are as follows:

1. Make changes to the appropriate keymap file
2. Save the file
3. `make clean`
4. Press the reset button on the PCB/press the key with the `RESET` keycode
5. `make <arguments> dfu` - use the necessary `KEYMAP=<keymap>` and/or `COMMON=true` arguments here.
## 3. Beyond the Basics
### 3.1 Backlight Control
### 3.2 Magic Key
### 3.3 Mouse Keys
### 3.4 Custom Functions
