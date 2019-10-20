# FIND PLACE FOR OR DELETE #
- **alt+left click** - loop selection. In Edit Mode lets you select entire LL chain or faces or edges.

# Customizations Employed #
- **Turn Off Auto Perspectiive** - Edit > User Preferences > Navigation > Uncheck Auto Perspective
- **Loops Tools** - Just a ton of really useful tools, shown by right clicking in Edit mode, or by hitting n > Edit.
- **UV Squares** - https://github.com/Radivarig/UvSquares was manually installed to help square up UV un-wraps. Not sure how critical it actually is though.

# General ViewPort Controls #
- **Unit Changes** = Got to the "Scene" tab under properties and then Unit Systems, Length to whatever you need.
- **Floor Unit Detail** - By Default, with Inch Units the Floor grid lines are spaced 1 FOOT apart. To change to inch lines, you adjust the scale via the "Viewport Overlays" drop-down in the top right to 1/12.
- **New ViewPorts/Multiple Windows** = Put mouse in any corner of the viewport intended to be split (until crosshairs is shown). Click and drag in the direction the split is desired in.
- **ViewPort Swapping** - Holding Ctrl and clicking in the corner of the window and then dragging that window to another one will swap the contents of the windows.
- **Eliminating ViewPorts/Multiple Windows** = Right click the split between the windows and do "Join Area," then click the region you want to be coverd up/removed.
- **Quad View** = Ctrl+Alt+q toggles quad view on/off. Quad View is the 4 main views together. The toggling is super convenient.
- **Home** = adjusts zoom to show all.
- **~** = quick view select.
- **Alt+Middle Mouse** = Hold Alt and Drag Direction w/ Middle Mouse to align view that way.
- **Num-pad** = toggles to pre-programmed views.
- **Tab** = Toggle between Object and Edit mode. Does edit mode for selected object/objects.
- **Shift+Right Click** = Moves 3d Cursor to that Point.
- **Shift+a** = Adds mesh primitives at the 3d Cursor.
- **n** = Hide/Shows properties of current element (on the right of viewport). Works in and out of Edit Mode, so on macro object level to most micro vertex level.
- **t** = Hide/Shoes tools on the left. Works in and out of Edit Mode.
- **Infinite Screen** = remember the mouse will rollover from edge to edge so you can continue scaling for example.

# Selection #
- **a** = selects all.
- **w** = toggles the selection method: Select, Box, Circle, Lasso. This works in and out of Edit Mode.
- **c** = Circle select. Scroll wheel to adjust size. Right click to exit with select. This works in and out of Edit Mode.
- **Alt+a** = deselects all.
- **Ctrl+i** = Inverts selection.

# Collections and Oultiner Manipulation #
- **m** - While in Object mode, if you have object/s selected, it will offer to move those objects to a different or new collection group.
- **Shift+m** - Allows you to make make a link to the selected object in a another collection. Goal being that you could have one object in two different collections if it helped you organize your work.
- **Restriction Toggles** - The filter symbol in the top right is very useful for showing the "Selectable" button next to each object. Very useful for making an object unselectable when you don't want to snap to it for example.
- **Parents** - To make one object a parent of the other (so that if the parent moves the child follows for example), you select the child, then the parent, and thhen hit Ctrl-p and then "Object (Keep Transform). It will move the object in the outliner under the parent object. 

# Visibility #
- **h** = hide selected. Visibility affects feature creation like edge loop construction and l-selection. Meaning hiding things temporarily is extremely useful. If you are propoprtionally edititing a group of vertices hidden vertices won't move even.
- **alt+h** = show all temporarily hidden items.
- **Shift+h** = hide all but selected.
- **Clipping** = you can adjust where your view gets clipped by hitting n to get the properties side bar then hit the View tab and change 'Clip Start'
- **z** = View Mode selection wheel. Different presentation of what's in top-right.
- **Alt-z** = X-ray. Show Whole Scene Transparent. Limits selection to what's visible or all. Also, button in the top right of viewport.
- **Shift-z** = Wireframe mode toggle.
- **.** - Focuses selected item on the screen i.e. Adjusts zoom so that selection fills screen. On numpad.
- **5** - Toggles perspective on/off. On numpad.

# Common Commands (Edit and Object Mode) #
- **l** = selects linked elements. Works for vvertices, edges, surfaces. etc. Even works in UV editing.
- **x** = deletes current selection.
- **Shift+d** = duplicate element. Common in every mode.
- **g** = Grab. Move in view. Then pressing X,Y,Z moves only in that direction (otherwise in view plane). **Middle clicking hot swaps the axis lock to whichever way you've dragged throughout typical operations**
- **s** = Scale selection. Can specify axis using X,Y,Z as expected and can avoid scaling along axis with Shift+X,Y,Z. Nice hack to put elements at same x coordinate is selecting them both and scaling only in x then hitting 0. Just make sure the "Pivot Point" is set to active element and the point you don't want to move is the last one you select.
- **r** = Rotates object. Amount back be typed into properties explicitely. X,Y,Z to contrain rotation works.
- **.** = shows menu to select where to rotate/scale/manipulate about. This is the quick select of the "Pivot Point" drop down at the top of the window. Active element is very useful because then the operation will occur about that element. It's just the last element you select, marked in white. So if you select a cloud of vertices, you can shift de-select a point and reselect it to make it white/active.
- **Shift** - while moving or manipulating anything holding shift makes the movements smaller/more precise.
- **Ctrl** - while rotating elements holding control makes the rotation snap to 5° increments.
- **Shift+s** = Snap-To Menu. Can snap cursor to objects or objects to cursor.
- **Shift+r** = Redo. Nice for building evenly spaced grids while in edit mode (for example, after extruding an edge), or to make evenly spaced objects (for example, after making a duplicate object to the side of the original).

# Edit Mode Specific Commands #
- **1,2,3** = Selection limits vertices, edges, faces.
- **Ctrl+x** = dissolve vertex, edge, or face, instead of normal x = delete.
- **gg** = Grab, with Slide. Either Edge Slide or Vertex Slide depending on selection mode. Primary use is redistributing things - intelligently. Otherwise, Useful to simplify edges and then use Remove Doubles.
- **c** - Constrains the movement of edge slide at the angle. Useful for allowing you to effectively edge slide extend using gg or e > drag inboard a bit > c > then drag outboard to extend.
- **Snap** = extremely useful with snap vertices to faces in the background (for example when retopologizing a scan). If you hit the magnet button in the top-right, it turns on "use_snap". Furthermore, you can then turn on "Project Individual Elements" in Blender 2.8 you can select a bunch of vertices and move them and they'll project along the view direction onto whatever face is back there. Useful for getting a bunch of vertices snapped quickly by selecting them, hitting "g", and immedietely hitting Enter. If it's turned off, you cant temporarily use snap while moving vertices by holding Ctrl.
- **Alts+s** = Scale along normals. When used on faces effectively moves face long normal of the face.
- **o** = Toggles On/Off proportional editing. There are different types. Remember that hidden elements aren't effected by proportional edits.
- **Scroll Wheel During Edit** = Adjusts Area of Effect.
- **Limit Regions Affected By Proportional Modelling** = Note hidden geometry won't be affected by proportional edits (h, alt+h).
- **e** = extrude in direction.
- **i** = inserts face inboard.
- **Ctrl+e** = Edge Features (Also available at top under the "Edge" Button).
- **Ctrl+f** = Face Features (Also available at top under the "Face" Button).
- **u** = UV Features (Also available at top under the "UV" Button).

## Vertex Specific Controls ##
- **Alt-m** = Combine vertices (provides options for combine to first, to last, to middle, collapse multiples.
- **Alt-m > By Distance** = removes overlapping vertices from SELECTION (often created while simplifying edges with gg moves). Rememeber that it removes them from selection, so often start by hitting 'a' first. Previously, in Blender 2.79 it was Mesh > Vertices > Remove Doubles.
  - Alternate Method 1 =  Mesh > Clean-Up > Merge By Distance at the top of window.
  - Alternate Method 2 = Right Click > Merge Vertices > By Distance.

# Edge-Specific Controls #
- **Ctrl+r** = Loop Cut / Sub-Divide. Click once desired orientation is achieved, then adjust position and click to accept. Works on both edges and faces. Right clicking places it right in the middle.
  - **Scroll Wheel** - Adjusts number of loop cuts. Can type a number to dictate number of sub-divides.
  - **e** = choose one specific edge (of the two) to inform the loop cut geometry instead of informing the shape by proportionally based on which edge it's closest to.
  - **f** = toggles which of the edges is being explicitely used to inform the loop cut profile.
- **k** = knife tool. Cuts faces and puts down edges. Manual version of loop-cut... ish. Hitting "c" while in this mode "constrains" the tool to certain angles (vertical, 45°, and horizontal).
- **f** = makes edge or face based on the vertex selection (2 = edge, greater than 2 = face). Effectively merges faces if you select multiple faces then hit f.
- **Shift+e** = crease adjustment. Sharpens and makes line red. Avoid unless it's a really simple quick object. Better off using bevel probably. Reason's being: 1) Crease is Blender specific. So if you use it you can't export it and pass cleanly to other programs the way a pre-sub-d's mesh can be (they'd then apply sub-d in their program). 2) creases use the length of the edges to inform, so if the edges are different lengths extending away from the actual break you are trying to sharpen the sharpness will vary unintentionaly.

# Surfacing #
- **Creating First Curves** - Shift-a > Plane, then edit the plane and do Mesh > Delete > Edge Collapse in the top menu (just hitting 'x' and then Edge Collapse is easier to get the plane to collapse to a single Vertex. Then it's possible to extrude it to create edges. Other option is to Shift-a anymesh object, Edit-Mode it, delete everything out of the object and then use shift + right click to start adding points. If you have a vertex selected it will continue to add them in a edge chain. If you deselect everything it will just add a vertex by itself.
- **Background Images** - Shift-A > Image > Background. Another simple way to do it is to just drag the image into the Blender window. It'll add them as an Empty object that you can then modify. It'll drop it in normal to your current view, so either go to one of the principle views first, or clean up the Transform properties afterwards.
  - To control whether the image is shown on the front or back of whatever view plane it was on, go into the `Object Data` tab in `Properties` and select the appropriuate "Side". That's nice to use when you have both a top and bottom view, or a front and rear view.
  - Additionally, the `Depth` setting let's you control whether the lines are 
  - `Use Alpha` makes things more legible and lets you turn adjust some transparency.
- **Smooth Shading** - Compared to previous versions of Blender, in 2.8 to use Smooth Shading, you select the object and the hit the Object menu at the top and "Shade Smooth." It's also available if you just right click over the object.
- **Bevel** - Good way to reinforce edges is by adding a Bevel with Ctrl-b. Setting segments to 2 puts edge loops on either side of your original curve.
- **Merging Vertices** - It's sometimes useful to do minor retopogizing using 'gg' and sliding vertices on top of each other. Then you can select all and do "Vertex" > "Remove Double Vertex" at the top to remove the duplicate. Another option is to do that Double Vertex Removal automatically by turning on the "AutoMerge Editing" in the Mesh Options at the very top right of the Blender 2.8 window. The former is likely the most useful for intermittent usage since people usually turn off "AutoMerge Editing" since it can cause issues otherwise.
- **Recalculate Normals** - If visibilities ever get screwy (likely from making floating faces while editing a body, you can recalculate all the normals using Mesh -> Normals -> Recalculate Outside. It's in the Mesh menu at the top of Edit Mode in 2.8. Otherwise, shortcut for it is *shift+n*.
Mesh -> Normals -> Recalculate Outside. 

# Object Manipulations #
- **Join** - It's possible to joing together two seperate objects using ctrl-j.
- **Seperate** - While in edit mode you can select the region you want to seperate and use "p" to break it out into a seperate object.
- **Append** - If there's an object in another Blender file you want, you can add it to your current file using File > Append.

# Sculpt Mode #
- **f** = adjust size of Sculpting Brush.
- **Shift+f** = adjust strength of the Sculpting Brush. 
- **ctrl** = alternates the effect of the sculpting brush. If adding a bulge, holding ctrl while using the brush will create dent (or vice versa).
- **shift** = while in a brush mode, regardless of it's type, changes the brush temporarlity to smooth.
- **Dyntopo** - can be toggled in the top menus, but the true settings are in the "Active Tool and Workspace Settings" pane in the "Properties" window on the RH-side (by default). That's where you can change detail size (and other useful more granular settings).
- **Symmetry** - The viewport has an X,Y, and Z button in the top-right that indicates what symmetry is being maintained throughout sculpting.

# Camera Manipulations #
- **0** - on numberpad shows you what the camera's seeing. You can select the camera frame and do g to move the camera around. If you want to zoom, hit g and then the middle mouse buttin and then move mouse up and down. Another option for reproducing that behaviour is to hit n and under the View tab, check the "Lock Camera to View." Then as you fly around the camera moves with you.
- **Ctrl+Alt+0** - snaps the currently selected camera to your current view. Super useful, just make sure you have the camera selected or you'll be confused.

# Rendering #
- **F12** - renders current scene.
- **F11** - brings up previous render.
- **Shift-s** - save render menu.
- **Exposure Judgement** - Under the Render Properties list the bottom-most group is "Color Management", turning the View Transform section to "False Color." Appropriately exposed elements are gray-ish. Decrease the lighting strength until the critical elements are gray.
- **De-Noiser** - To use the new Intel AI denoiser, go to the Layer tab in the properties and check the "Denoising Data" option, then do another render, and go to the Compositor tab (which is everything that happens after render is finished). Sometimes you'll have to check "Use Nodes" to get things to show up. Then you can wire in a "Denoiser" node that wires the Noisy Image, Denoising Normal, and Denoising Albeto into it and then then the result into the Composite node. This is referenced here briefly: https://www.youtube.com/watch?v=5lr8QnR5WWU

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

# Viewing PLY Colors in Blender 2.8 #
Blender 2.8 removed the normal way for seeing Vertex Colors of ply files: adding a new material and checking the "Vertex Color Paint" box under Material > Options. In Blender 2.8, the easiest way to do it is to plug the Vertex Colors in the Color input of the shader. It's possible to do that by adding a meterial to the object and then do Shift-a > Input/Vertex Colors/Col and wire those colors into the Shader.
