# Customizations Employed #
- **Turn Off Auto Inspection** - Edit > User Preferences > Navigation > Uncheck Auto Perspective

# General ViewPort Controls #
- **Tab** = Toggle between Object and Edit mode. Does edit mode for selected object/objects.
- **New ViewPorts/Multiple Windows** = Put mouse in any corner of the viewport intended to be split (until crosshairs is shown). Click and drag in the direction the split is desired in.
- **ViewPort Swapping** - Holding Ctrl and clicking in the corner of the window and then dragging that window to another one will swap the contents of the windows.
- **Eliminating ViewPorts/Multiple Windows** = Right click the split between the windows and do "Join Area," then click the region you want to be coverd up/removed.
- **Home** = adjusts zoom to show all.
- **~** = quick view select.
- **l** = selects linked vertices. Works for edges and surfaces too. Even works in UV editing.
- **a** = selects all.
- **c** = Circle select. Scroll wheel to adjust size. Right click to exit with select.
- **Alt+a** = deselects all.
- **Shift+i** = Inverts selection.
- **Infinite Screen** = remember the mouse will rollover from edge to edge so you can continue scaling for example.
- **Unit Changes** = Got to the "Scene" tab under properties and then Unit Systems, Length to whatever you need.
- **Shift+a** = Adds mesh primitives at the 3d Cursor.

# Edit Mode #
- **1,2,3** = Selection limits vertices, edges, faces.
- **w** = toggles the selection method: Select, Box, Circle, Lasso.
- **Show Whole Scene Transparent/XRay Button** = limits selection to what's visible or all. Button in the top right of viewport.
- **n** = Hide/Shows properties of current element (on the right of viewport).
- **t** = Hide/Shoes tools on the left
- **h** = hide selected.
- **alt+h** = show selected.
- **x** = delete. Works everyplace.
- **Shift+d** = duplicate element.
- **g** = Grab. Move in view. Then pressing X,Y,Z moves only in that direction (otherwise in view plane). **Middle clicking hot swaps the axis lock to whichever way you've dragged throughout typical operations**
- **gg** = Grab, with Slide. Either Edge Slide or Vertex Slide depending on selection mode. Primary use is redistributing things - intelligently. Otherwise, Useful to simplify edges and then use Remove Doubles.
- **s** = scale. Can specify axis using X,Y,Z as expected and can avoid scaling along axis with Shift+X,Y,Z.
- **Alts+s** = Scale along normals. When used on faces effectively moves face long normal of the face.
- **r** = Rotates object. Amount back be typed into properties explicitely. X,Y,Z to contrain rotation works.
- **.** = shows menu to select where to rotate about.
- **Scroll Wheel During Edit** = Adjusts Area of Effect.
- **o** = Toggles On/Off proportional editing.
- **Limit Regions Affected By Proportional Modelling** = Hidden geometry won't be affected by proportional edits (h, alt+h).
- **e** = extrude in direction.
- **i** = inserts face inboard.
- **Shift+s** = Snap to Menu. Can snap cursor to objects or objects to cursor.
- **Ctrl+e** = Edge Features (Also available at top under the "Edge" Button).
- **Ctrl+f** = Face Features (Also available at top under the "Face" Button).
- **u** = UV Features (Also available at top under the "UV" Button).

# Vertex Specific Controls #
- **Alt-m** = Combine vertices (provides options for combine to first, to last, to middle, collapse multiples.
- **Mesh > Vertices > Remove Doubles** = cleans up overlap vertices (often created while simplifying edges with gg moves).

# Edge-Specific Controls #
- **Ctrl+r** = Loop Cut / Sub-Divide. Click once desired orientation is achieved, then adjust position and click to accept. Works on both edges and faces.
  - **Scroll Wheel** - Adjusts number of loop cuts.
  - **e** = choose one specific edge (of the two) to inform the loop cut geometry instead of informing the shape by proportionally based on which edge it's closest to.
  - **f** = toggles which of the edges is being explicitely used to inform the loop cut profile.
- **k** = knife tool. Cuts faces and puts down edges. Manual version of loop-cut... ish.
- **f** = makes face, effectively merges faces if you select multiple.
- **Shift+e** = crease adjustment. Sharpens and makes line red. Creases use the length of the edges to inform, so if the edges are different lengths extending away from the actual break you are trying to sharpen the sharpness will vary unintentionaly.

# UV Unwrapping #
- Before unwrapping anything you have to be in edit mode for the object and select the faces you want to unwrap, likely by hitting a to select all of them.
- **Smart UV Project** = automated UV Unwrapping. Bring up using "u" shortcut.
- **Ctrl+e -> Mark Seam** = Manually define a seam (in red) along that selected edge.
- **Clear Seam** = Obviously stops the edge from being a seam for unwrap.
- **Unwrap** = Unwrap Mesh using defined seams. Bring up using "u" shortcut.
- **General UV Editor Notes**
  - Middle Click = pan
  - Scroll = zoom in and out.
  - a = still selects all.
  - g = still moves around.
  - l = still selects all linked geometry.
  - Display Tab -> Stretch = shows you how much the texture is stretched on each face (and likely where you need to seam more).
  - Sync = syncs your selection between the mesh viewport and the UV editor viewport so you can see where the unwrapped object is on your actual mesh.
  
# Texture Painting #
  - Start with smart UV project because you aren't putting an image on the mesh surface. You'll be painting on it and create the image effectively live, so the splits don't matter. It's mapped correct and just locked.

# Sculpt Mode #
**f** = adjust size of Sculpting Brush.
**Shift+f** = adjust strenght of the Sculpting Brush. 
**ctrl** = alternates the effect of the sculpting brush. If adding a bulge, holding ctrl while using the brush will create dent (or vice versa).
**shift** = while in a brush mode, regardless of it's type, changes the brush temporarlity to smooth.
**Dyntopo** - can be toggled in the top menus, but the true settings are in the "Active Tool and Workspace Settings" pane in the "Properties" window on the RH-side (by default). That's where you can change detail size (and other useful more granular settings).

# Surfacing #
**Creating First Curves** - Shift-a > Plane, then edit the plane and do Mesh > Edge Collapse to get the plane to collapse to a single Vertex. Then it's possible to extrude it to create edges. Other option is to Shift-a anymesh object, Edit-Mode it, delete everything out of the object and then use shift + right click to start adding points. If you have a vertex selected it will continue to add them in a edge chain. If you deselect everything it will just add a vertex by itself.
**Background Images** - Shift-A > Image > Background. Another simple way to do it is to just drag the image into the Blender window. It'll add them as an Empty object that you can then modify. To control whether the image is shown on the front or back of whatever view plane it was on, go into the Object Data tab in Properties and select the appropriuate "Side". That lets you put the front and back blueprint on the front plane and seeing the one that's relevant to the current view.
**Smooth Shading** - Compared to previous versions of Blender, in 2.8 to use Smooth Shading, you select the object and the hit the Object menu at the top and "Shade Smooth."
**Bevel** - Good way to reinforce edges is by adding a Bevel with Ctrl-b. Setting segments to 2 puts edge loops on either side of your original curve.
**Merging Vertices** - It's sometimes useful to do minor retopogizing using 'gg' and sliding vertices on top of each other. Then you can select all and do "Vertex" > "Remove Double Vertex" at the top to remove the duplicate. Another option is to do that Double Vertex Removal automatically by turning on the "AutoMerge Editing" in the Mesh Options at the very top right of the Blender 2.8 window. The former is likely the most useful for intermittent usage since people usually turn off "AutoMerge Editing" since it can cause issues otherwise.
**Recalculate Normals** - If visibilities ever get screwy (likely from making floating faces while editing a body, you can recalculate all the normals using Mesh -> Normals -> Recalculate Outside. It's in the Mesh menu at the top of Edit Mode in 2.8. Otherwise, shortcut for it is *shift+n*.
Mesh -> Normals -> Recalculate Outside. 

# Object Manipulations #
**Join** - It's possible to joing together two seperate objects using ctrl-j.
**Seperate** - While in edit mode you can select the region you want to seperate and use "p" to break it out into a seperate object.
**Append** - If there's an object in another Blender file you want, you can add it to your current file using File > Append.

# Viewing PLY Colors in Blender 2.8 #
Blender 2.8 removed the normal way for seeing Vertex Colors of ply files, adding a new material and checking the "Vertex Color Paint" box under Material > Options. In Blender 2.8, the easiest way to do it is to plug the Vertex Colors in the Color input of the shader. It's possible to do that by adding a meterial to the object and then do Shift-a > Input/Vertex Colors/Col and wire those colors into the Shader.
