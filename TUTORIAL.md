# Writing custom keyboard firmware using Jack Humbert’s fork of TMK
## 1. Introduction
Welcome! The purpose of this tutorial is to take someone knowing nothing about writing custom firmware for a keyboard and give them the tools to write one for *Jack Humbert’s fork of tmk_keyboard.* Note that last part carefully, what is written here is written for [Jack Humber's fork of TMK](https://github.com/jackhumbert/tmk_keyboard), and will **only** work with this version of tmk_keyboard.

This tutorial is split into two sections: ‘Basics’ and ‘Beyond the Basics.’ By following the ‘Basics’ section you will be taken from a blank file to the uploading of your firmware. This might very well be all you need. The ‘Beyond the Basics’ section deals with more advanced topics.

### 1.1 Method
The ‘Basics’ section of this tutorial will be done in the style of following the implementation of *a particular* keymap from start to finish. This is a keymap for a Planck ortho—linear keyboard. This has three advantages:
* Programmable keyboards offer, by definition, an enourmous array of possible configurations and uses. The task of outlining all of them would be impossible. Nonetheless the principles involved in any one implementation are *transferrable* to any other. By following *one* implementation you will gain the skills to write virtually any customisation.
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

**Jack, I was thinking about canabalising your existing [Tutorial](https://github.com/jackhumbert/tmk_keyboard/blob/master/doc/build.md) for this part. I'll post credit at the end naturally, but are you okay with that?**

#### 2.1.1 Summary
In this section you will download the appropriate version of the tmk_keyboard firmware as well as dfu_programmer and operating system specific development tools so that you have everything you need compile and transfer (flash) the firmware. You will then verify the installation.

#### 2.1.2 Downloading the Software

#### 2.1.3 Verifying

### 2.2. Keymaps
\\ TODO check on priority of higher layers (neccesity of TRNS).
#### 2.2.1 Summary
In this section you will create a new keymap file, add the `#include "extended_keymap_common.h"` Statement to the beginning, and write the default layer of the keyboard within a `const uint16_t PROGMEM keymaps[][MATRIX_ROWS][MATRIX_COLS] = { };` statement as a series of rows delimited between braces and separated by commas (except for the last) and a series of colums delimited by commas within those braces.

#### 2.2.2 Creating a keymap file

Using your favorite text editor create a new file with the name `<yourkepmap.c>` in the 'planck' or 'quark' folder of your tmk installation. In other words, if you are in the tmk firmare directory go the folder `keyboard/planck/` or `keyboard/quark`. You should replace the 'yourkeymap' part of `<yourkeymap.>` with whatever name you want.

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

At the top is a section which is commented out, this means that the software that converts this file into a form ready to send to the keyboard will ignore it. It is there so we can write in a keymap using just the alphanumeric symbols we are used to.

##### 2.2.3.1 The Alphabet

I am a qwerty typist, so lets fill in the alphabet:

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

You might not like the above layout. Feel free to swap things around

##### 2.2.3.2 MIT and Grid Layout 

You will notice that inside the curly braces after `const uint16_t PROGMEM keymaps[][MATRIX_ROWS][MATRIX_COLS] = { };` there are a series of rows delimited between braces and separated by commas (except for the last row). Inside the curly braces that make up each row are a series of colums delimited by commas within those braces. You adjust these parameters according to the lay of the keyboard. I have shown a keymap for an MIT layout. But what about a grid? Simple, you just add another comma to the bottom row. So we get:

```
  [0] = {
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     }, 
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     },
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     },
    {    ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     ,     }
  },
```
##### 2.2.3.3 The Full Keyset

##### 2.2.3.4 Layers

### 2.3. Functions
### 2.4. Macros
### 2.5. Compliation and Flashing
## 3. Beyond the Basics
### 3.1 Backlight Control
### 3.2 Magic Key
### 3.3 Mousekeys
### 3.4 Custom Functions
