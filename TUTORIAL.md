# Writing custom keyboard firmware using Jack Humbert’s fork of TMK
## 1. Introduction
Welcome! The purpose of this tutorial is to take someone knowing nothing about writing custom firmware for a keyboard and give them the tools to write one for *Jack Humbert’s fork of tmk_keyboard.* Note that last part carefully, what is written here is written for https://github.com/jackhumbert/tmk_keyboard, and will only work with this version of tmk_keyboard.

This tutorial is split into two sections: ‘Basics’ and ‘Beyond the Basics.’ By following the ‘Basics’ section you will be taken from a blank file to the uploading of your firmware. This might very well be all you need. The ‘Beyond the Basics’ section deals with more advanced topics.

### Method
The ‘Basics’ section of this tutorial will be done in the style of following the implementation of *a particular* keymap from start to finish. This is a keymap for a Planck ortho—linear keyboard. This has three advantages:
* Programmable keyboards offer, by definition, an enourmous array of possible configurations and uses. The task of outlining all of them would be impossible. Nonetheless the principles involved in any one implementation are *transferrable* to any other. By following *one* implementation you will gain the skills to write virtually any customisation.
* You will see the principle of iterative refinement at work, which is a part of any project of this kind.
* By following the tutorial and at each step copying what is done modified for your own requirements you can come out the other end with a finished product that is very close to—if not exactly—what you want. It is therefore recommended that you **follow and adapt** your requirements as they go along.

There is one main disadvantage:
* You will not see all functions in use.

You will see a great many functions, however, and those I don’t use will be explained where appropriate. The skills you pick up should enable you to use these without difficulty. 

### Layout
Each section is numbered so that I can refer you forward or backwards easily. I begin each major section with a summary of what that section covers. This accomplishes three things.
* It gives you an idea of what is about to happen. The human brain processes information better if it has a structure to hang the new material in.
* You can read this and see whether you need the information that section gives.
* It also serves as a reminder should you quickly need to refresh the information.

In doing this is am shamelessly copying ancient and medieval authors whose tables of contents and section introductions are a standard to aspire to (if you’re interested look at Augustine’s *On the Trinity*, for example).

### Target Audience
Who is this tutorial for? Well, you are programming custom firmware for a mechanical keyboard so I assume that you have basic technical skills. To make it through the ‘Basics’ section—all you are likely to need—you should be able to edit text files, and run commands in a terminal. That’s it. After that there are one or two things in the ‘Beyond the Basics’ section which require some c.

### Disclaimer
This information is provided as a guide and every effort has been made to make sure it is correct, but it will not be perfect. Use it at your own risk.

## 2. The Basics
### 2.1 Installing the Right Tools
#### Summary
In this section you will download the appropriate version of the tmk_keyboard firmware as well as dfu_programmer and operating system specific development tools so that you have everything you need compile and transfer (flash) the firmware. You will then verify the installation.

#### Downloading the Software

#### Verifying

### 2.2. Keymaps
\\ TODO check on priority of higher layers (neccesity of TRNS).
#### Summary
In this section you will create a new keymap file, add the `#include "extended_keymap_common.h"` statement to the beginning, and write the default layer of the keyboard within a `const uint16_t PROGMEM keymaps[][MATRIX_ROWS][MATRIX_COLS] = { };` statement as a series of rows delimited between braces and separated by commas (except for the last) and a series of colums delimited by commas within those braces.

### 2.3. Functions
### 2.4. Macros
### 2.5. Compliation and Flashing
## 3. Beyond the Basics
### 3.1 Backlight Control
### 3.2 Magic Key
### 3.3 Mousekeys
### 3.4 Custom Functions
