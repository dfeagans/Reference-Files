#General ViewPort Controls#
**Tab** = Toggle between Object and Edit mode. Does edit mode for selected object/objects.
**New ViewPorts/Multiple Windows** = Put mouse in any corner of the viewport intended to be split (until crosshairs is shown). Click and drag in the direction the split is desired in.
**Eliminating ViewPorts/Multiple Windows** = Right click the split between the windows and do "Join Area," then click the region you want to be coverd up/removed.
**Home** = adjusts zoom to show all.
**~** = quick view select.
**l** = selects linked vertices. Works for edges and surfaces too. Even works in UV editing.
**a** = selects all.
**c** = Circle select. Scroll wheel to adjust size. Right click to exit with select.
**Alt+a** = deselects all.
**Shift+i** = Inverts selection.
**Infinite Screen** = remember the mouse will rollover from edge to edge so you can continue scaling for example.
**Unit Changes** = Got to the "Scene" tab under properties and then Unit Systems, Length to whatever you need.


# Edit Mode#
**1,2,3** = Selection limits vertices, edges, faces.
**w** = toggles the selection method: Select, Box, Circle, Lasso.
**Show Whole Scene Transparent/XRay Button** = limits selection to what's visible or all. Button in the top right of viewport.
**n** = Hide/Shows properties of current element (on the right of viewport).
**t** = Hide/Shoes tools on the left

**g** = Grab. Move in view. Then pressing X,Y,Z moves only in that direction (otherwise in view plane). **Middle clicking hot swaps the axis lock to whichever way you've dragged throughout typical operations**
**gg** = Grab, with Slide. Either Edge Slide or Vertex Slide depending on selection mode. Primary use is redistributing things intelligently. Otherwise, Useful to simplify edges and then use Remove Doubles.
**s** = scale. Can specify axis using X,Y,Z as expected and can avoid scaling along axis with Shift+X,Y,Z.
**r** = Rotates object. Amount back be typed into properties explicitely. X,Y,Z to contrain rotation works.
**.** = shows menu to select where to rotate about.
**Scroll Wheel During Edit** = Adjusts Area of Effect.
**o** = Toggles On/Off proportional editing.
**Shift+s** = Snap to Menu. Can snap cursor to objects or objects to cursor.
**Ctrl+e** = Edge Features (Also available at top under the "Edge" Button).
**Ctrl+f** = Face Features (Also available at top under the "Face" Button).
**u** = UV Features (Also available at top under the "UV" Button).

# Vertex Specific Controls #
**Alt-m** = Combine vertices (provides options for combine to first, to last, to middle, collapse multiples.
**Mesh > Vertices > Remove Doubles** = cleans up overlap vertices (often created while simplifying edges with gg moves).

# Edge-Specific Controls #
**Ctrl+r** = Loop Cut / Sub-Divide. Click once desired orientation is achieved, then adjust position and click to accept. 
  - **e** = choose one specific edge (of the two) to inform the loop cut geometry instead of informing the shape by proportionally based on which edge it's closest to.
  - **f** = toggles which of the edges is being explicitely used to inform the loop cut profile.
**k** = knife tool. Cuts faces and puts down edges. Manual version of loop-cut... ish.
**Shift+e** = crease adjustment. Sharpens and makes line red.
