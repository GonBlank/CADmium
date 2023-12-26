# CADmium

This is an attempt to create a new CAD program from scratch. Legacy CAD programs have taken many thousands of years of collective engineering time to get where they are so this program will never be able to compete on breadth of features. CADmium is intended to capture 80% of the most common CAD use cases while doing less than 20% of the work. For now I am targetting the home hobbyist who just wants to design a widget for their 3D printer, not a company that wants to design a car or airplane.

Features:

- Simple, modern, parametric CAD UI that runs in a browser
- Export as .step, .obj, or .cadmium (a json-based CAD format that this project is inventing)
- Functions without an internet connection (once you've loaded the page)

**Status**: Early prototype. This tool is not yet an MVP, but is being developed in the open.

## Technology

The boundary representation engine under the hood is [truck](https://github.com/ricosjp/truck), which is written in rust and is not dependent on any legacy b-rep engine.

Leveraging truck, I wrote a small rust library called [cadmium](https://github.com/MattFerraro/CADmium/tree/main/src/rust/cadmium) which provides structs for projects, workspaces, sketches, extrusions, and constraints. My goal is that this rust library provides all the same functionality as the UI for anyone who prefers code-first CAD. This library is able to save and load projects to disk as json. I have also built a set of javascript bindings so that the whole thing can be compiled to wasm and run in a browser.

The UI is built with SvelteKit and Tailwind. It is [hosted](https://cadmium-nine.vercel.app/tailwind) with Vercel. I use [three.js](https://threejs.org/) for rendering, which in this case uses WebGL under the hood. I use [Threlte](https://github.com/threlte/threlte) to manage the scene graph.

## Licensing

The entire project is available under the MIT license, but I can't guarantee that it will be forever. I would like to make money at some point in the future.

## TODO for MVP (0.1 Release)

I am currently racing toward an MVP, which I expect in February, 2024. To reach it, I still need:

- [ ] Sketching
  - [ ] Ability to create a sketch on the face of a solid
  - [ ] New Rectangle Tool
  - [ ] Ability to select and delete points/lines/circles/constraints
  - [ ] Ability to create and modify constraints
  - [ ] Automatic solving of constraints (hide the step/solve buttons)
  - [ ] Handle point on line constraints and resulting face geometry
- [ ] Extrusion
  - [ ] Configure an extrusion to subtract, add, or create new solids
  - [ ] Configure extrusion depth
  - [ ] Stabilize face identification so that it doesn't jump around when you make sketch changes
- [ ] Revolution
  - [ ] Same as extrusion
- [ ] Project
  - [ ] Ability to rename the project
  - [ ] Ability to create and delete entire workbenches
  - [ ] bind ctrl + s to .cadmium export, and ctrl + o to .cadmium import
  - [ ] Default to completely empty project
  - [ ] Constantly save .cadmium file to localStorage, and offer to re-open the last saved project in case you accidentally close the tab
  - [ ] Natural UX for when a link truly breaks
  - [ ] Orientation cube in upper right (replace Gizmo)
  - [ ] Label .cadmium projects as
- [ ] Export
  - [ ] proper scaling on .obj and .step exports (currently the units are off)
  - [ ] .dxf export for sketches
- [ ] Marketing
  - [ ] Youtube video demonstrations
    - [ ] A simple cube
    - [ ] A plate with screw holes
    - [ ] Something pretty complex
  - [ ] A nice website
    - [ ] With Screenshots
    - [ ] Clear call to action
    - [ ] Show the Youtube videos
    - [ ] Create a Patreon or something
  - [ ] Social Media
    - [ ] Twitter Account
    - [ ] Discord

## TODO for Actual Product (1.0 Release)

- [ ] Sketching
  - [ ] Mirror
  - [ ] Fillet
  - [ ] Snip
  - [ ] Circular Pattern
  - [ ] Rectangular Pattern
  - [ ] Center Point Rectangle
  - [ ] Ellipse
  - [ ] 3 Point Circle
  - [ ] Convert to/from construction lines
  - [ ] Project points/lines from Solids
  - [ ] "Smart Constraints" which can be toggled on or off as you draw
- [ ] Extrude
  - [ ] Up to face
  - [ ] With draft angle
- [ ] Solids
  - [ ] Boolean (or Union, Intersection, Subtract)
  - [ ] Mirror
  - [ ] Delete
  - [ ] Transform (or Rotate and Translate?)
  - [ ] Chamfer
  - [ ] Fillet
- [ ] Assemblies
  - [ ] Fixed joint
  - [ ] Rotational joint
  - [ ] Translational joint
  - [ ] Cylindrical joint
  - [ ] Ball joint
  - [ ] Interference Detection
- [ ] Views
  - [ ] Toggle Orthographic/Perspective Camera
  - [ ] Disable/Enable edges
  - [ ] Disable/Enable shading
  - [ ] Section view
- [ ] User Preferences
  - [ ] Dark mode
  - [ ] Configure key and mouse bindings
  - [ ] Save your settings in localStorage
  - [ ] Export/import user settings as json files
- [ ] Electron or Tauri App so you can run local-only
  - [ ] Demonstrate associating .cadmium files with this app so they open on double-click from file explorer

## Features to Make Money

- [ ] SAAS - Hobbyist ($x/month)
  - [ ] Optional User accounts
  - [ ] Host files on behalf of users, so they can use any laptop at any time
    - [ ] Use Automerge under the hood so updates sync nicely across devices
    - [ ] Landing page that shows users the projects they have access to
  - [ ] Allow users to share projects with other individual users (read only or read-write)
  - [ ] Allow users to share projects into the public domain
  - [ ] Searchable catalog of public projects
  - [ ] Git integration
  - [ ] Thingiverse or Printables integration (one-click publish?)
- [ ] SAAS - Commercial ($y/month)
  - [ ] Groups which have a set of Group admins and set of Users, and can be the owners of Projects. Every user in the group can see and edit any Project owned by the Group.
  - [ ] Organizations which have a set of Org admins, a set of Users, and a set of Groups
  - [ ] Org admins can control Group membership, designate Group admins, transfer Projects between Groups
  - [ ] Group admins can only control Group membership
  - [ ] Clearance control: Annotate users with a Clearance level. Annotate Projects or Groups as requiring specific Clearance Levels or higher. Regardless of other rules, never allow a user to open a Project or join a Group they do not have sufficient Clearance for.
  - [ ] Disallow any Clearance Controlled projects from being shared publicly
  - [ ] Easy "revoke all access" button for Org Admins to remove employees who leave their companies
    - [ ] Support backups in case a User, Group Admin, or Org Admin causes wide damage: Must
  - [ ] Audit trail: Every time a User access a Project, or an Admin modifies a permission or Clearance, it gets logged in a way that not even Org Admins can modify, although all Org Admins can read the log
  - [ ] Set up a set of servers which exclusively run in [the US, Europe], to comply with certain [US, European] restrictions
- [ ] LLMs
  - [ ] Take every project that is shared in the public domain and (others which opt in) and train an LLM to predict steps given previous steps
  - [ ] Build a Co-Pilot like visual interface for using that LLM

## Might do, might not do

- [ ] Import
  - [ ] b-rep formats: .step, .x_t, .jt, .iges
  - [ ] mesh formats: .obj, .stl, .3mf, .gltf
  - [ ] project formats: .parasolid, .acis
- [ ] Add CAM functionality, or make a different, dedicated app
- [ ] Add FEA functionality, or make a different, dedicated app
