Template for DavidW's 'ui_add_portraits' function library. (This is a fully working mod that implements the library for Hannah's Portrait Pack.)

*****************
Overview
*****************

The idea here is that on EE games, the portraits are inserted into the game's core list of portraits, rather than just put into the 'custom portraits' folder. The advantages are:

(i) portraits can be separated by gender
(ii) portraits can be organized by race and class and not just lumped in at the end
(iii) we're better able to handle IWD-style use of different art in different places

The library also permits IWD2-style color matching, where the initially-assigned colors for your character match the colors of the portrait. (This is also back-implemented for all original-game portraits.)

(On non-EE games, we just copy portraits to the custom folder as per usual.)


***********************
Using ui_add_portraits
***********************

Firstly, you need to include the library and its dependencies. You need three files:
- ui_add_portraits.tpc (the core library along with bundled inline data)
- sfo_core.tph (a self-contained dump of DavidW's core function library)
- cd_portrait_copy.tph (CamDawg's portrait-management library, used on non-EE games)

The portraits should be listed in a 2DA table of form

	       skin	hair	major	minor	race		sex		class		disabled
EXAMPLE	   	INT	INT	INT	INT	human		f		fighter		no	

 (any of these columns can be missing, in which case defaults are assumed.) The function generates a master table of this form which lives in the 'shared' subdirectory of weidu_external (specifically, at weidu_external/data/shared/dw_portraits.2da)

 The first time the function is run, this table is created; also the UI is edited to include the needed color-preset changes (although they are inactive unless specifically activated; see below)

 'race' can be any of human, halfelf, elf, dwarf, halfling, gnome, halforc, special (the default).
 'class' can be fighter, wizard, cleric, thief, bard, barbarian, special (the default).
 'sex' can be f, m, fm (the default).
 'disabled' can be yes or no (the default).

 The bmps themselves should be named as follows:
 - EXAMPLEhires.bmp (for large high-resolution portraits) - copied to the L slot
 - EXAMPLE330.bmp - also copied to the L slot
 - EXAMPLEL.bmp - copied direct to the L slot
 - EXAMPLE269.bmp - copied to the M slot
 - EXAMPLEM.bmp - copied to the M slot

Once this is set up, just do this:

LAF ui_add_portraits
	STR_VAR portrait_path=[full path to the folders where the bmps live]
		portrait_table=[full path to the 2da with the portrait data]
END

***************************
Activating color matching
***************************

Just call this function: 
LAF activate_portrait_coloring END


***************************
Finding colors for a match
***************************

The four color entries needed in the 2da are *not* standard CRE file 0-255 colors: they're points on the color slider used in character creation/editing.

To find these, first run this function: 
LAF color_finder_tool END

(Tested on unmodded BG2EE, but should work fairly widely). This edits the (in-game) color customization page to display the numbers corresponding to each color. Load a savegame, go to customization->color, adjust the colors until they match your portrait, then make a note of the actual numbers.


