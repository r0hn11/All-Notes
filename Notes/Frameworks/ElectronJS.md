### What is ElectronJS
- Open source framework to build cross platform applications using web technologies
- Combination of chromium + NodeJS

### Advantages of Desktop app over Web app
- No conditional code for different browsers
- Less limited access to file systems
- Faster network connectivity

### Prerequisites 
- HTML
- CSS
- JavaScript
- NodeJS
- npm

### Getting started
- **[Documentation](https:\\www.electronjs.org\docs\latest\tutorial\quick-start)**
- **[Building first app](https://www.electronjs.org/docs/latest/tutorial/tutorial-first-app)**
- **Installing electron package**
	- `npm start` will execute the main.js file using `electron .` command (cannot be executed directly as electron is a dev dependacy) 
- **Package.json**
	- Initialize `package.json` manually or using `npm init`
	- - Install package as a **dev dependancy** using npm just like any other framework `npm i electron -D` or `npm install electron --save-dev`
	- Edit the scripts and other properties as below
	``` package_json
	{
	  "name": "packageName",
	  "version": "1.0.0",
	  "main": "main.js",
	  "devDependencies": {
	    "electron": "^21.1.1"
	  },
	  "scripts":{
	    "start":"electron .",
	  }
	}
	```
- **main.js**
	- **`main.js`** consists the functionality of the application framework (not the application). Consists methods and calls related to window, menu etc.
	- Import necessary packages i.e. `electron`,`path`,`url` etc.
	`const {app, electron, BrowserWindow}= require('electron');`
	- **Create a window**
		1. Create a window object. multiple window objects can be created for multiple windows
			- to use `node.js` functionalities in other `js` files, check `webPreferences` object passed to `BrowserWindow` (`nodeIntegration` & `contextIsolation` must be set)
		2. Load file from a url/path for the window object using `loadURL` or `loadFile`
		3. Close event for closing the window
		4. Call `createWindow` method when app is ready using an event (i.e. all initializing is done)
		- `createWindow()` can be defined for events like clicks also
``` main_js
	function createWindow(){
	// 1
	  win = new BrowserWindow({
		  height: 800,
		  width: 600
		  },
		  webPreferences: {
	        nodeIntegration: true,
	        contextIsolation: false
	    }
	  );

	// 2
	  win.loadURL(url.format({
	    pathname: path.join(__dirname,"ui/index.html"),
	    protocol: 'file',
	    slashes: true
	  }))
	  
	// win.loadFile('ui/index.html');

	// 3
	  win.on('close',()=>{
	    win = null;
	    console.log('Closed');
	  })
	}

	// 4
	app.on('ready',createWindow);
```
- **Adding files**
	- Add `index.html`,`css`,`js` files in a separate folder for clean view
	- In our case, we have added files in `ui` folder
	- we can either use **`loadFile`** to load a local file, or use **`loadURL`** to load data from that url
- Finally we can run main.js using command `npm start` (refer to scripts in `package.json`)

### Main and Renderer process
- Main process run by `main.js` is a parent process to start the application
- Renderer procecsses are invoked by main process to create a new window and subsequent functionalities
	- e.g. `main.js` runs main process. `index_one.js` runs process for `index.html`.
- Different processes will not interfere with each other even when they are invoked by within same process
	- e.g. if a `window2` is opened from `window1` then both will have different individual processes unless we mention the same. Parent process will not be inherited

### Browser window
**[Documentation](https://www.electronjs.org/docs/latest/api/browser-window)**
There are different types from which few are specified here. Refer to documentation to get complete walkthrough.
- A window is created for viewing the application
##### **Basic window** 
- Created using **`BrowserWindow`** class.
`let win = new BrowserWindow();`
##### Dimension window
- A window with specific dimensions is created
`let win = new BrowserWindow({ height:500px, width:500px, maxHeight: 600, minWidth: 300 });`
##### Colour window
- A default background colour can be set for a window
`let win = new BrowserWindow({backgroundColor:'#222'});`
##### Frameless window
- A window with no frame is created. No toolbar, no action buttons.
`let win = new BrowserWindow({ frame:false });`
##### Parent and child windows
- In case of parent and child windows, we can use following
`let parentwin = new BrowserWindow({ title:'Parent' });`
`let childwin = new BrowserWindow({ parent:parentwin, title:'Child' });`
- Note that child window will always overlap the parent window
- To restrict actions inside parent window before child window is closed, mention `modal:true` property for child window
##### ready-to-show
- The window is loaded only when the data is completely initialized.
- This avoids flashing windows
```
let win = new BrowserWindow({ show:false });
win.loadURL('https://github.com/')
win.once('ready-to-show', ()=>{
	win.show();
})
```
##### Creating a window based on existing window
- a new window is only needed to be created when there is no existing window.
```
app.on('activate',()=>{
	if(win === null){
		createWindow()
	}
})
```

### IPC (inter-process-communication)
- A **Renderer process**, as a browser window, can access **Native desktop API** such as ***file uploads, save files*** etc.
- This can cause **Resource leakage**
- To handle this security issue, electron provides **`IPC`** module

#### Implementation :
##### Asynchronous:
1. Inside **`index.html`**, create a button to trigger error window on click
```
<button id="err_async_">Error</button>
```
2. In **`renderer.js`**, import require modules and create an eventlistener for button. The method will send the `open-error-dialog` event to **main process** from the **renderer process**
```
const {ipcRenderer} = require(electron)

err_async_.addEventListener('click',()=>{
  ipcRenderer.send('open-error-dialog');
})
```
3. In **`main.js`**, we capture `open-error-dialog` event and generate a **dialog box** for display. Then we send another event `opened-error-dialog` back to **renderer process**
```
const {ipcMain, dialog} = require(electron)

ipcMain.on('open-error-dialog',(event)=>{
  dialog.showErrorBox('A custom error occured', 'This is a generated custom error. Chill.')
  event.sender.send('opened-error-dialog', 'Main process opened error dialog')
})
```
4. The `opened-error-dialog` event sent by **main process** is captured by **renderer process** inside **`renderer.js`**
```
ipcRenderer.on('opened-error-dialog',(event, args)=>{
  console.log(args);
})
```
**(Note**: event names can be changed)

##### Synchronous:
1. Inside **`index.html`**, create a button to trigger error window on click
```
<button id="err_async_">Error</button>
```
2. In **`renderer.js`**, add event to button considering necessary modules are already included. Event sends a synchronous message to **main process** 
```
errbtn_sync.addEventListener('click',()=>{
  const reply = ipcRenderer.sendSync('sync-msg');
}
```
3. In **`main.js`**, capture the event and reply in the same line
```
ipcMain.on('sync-msg',(event)=>{
  dialog.showErrorBox('A custom error occured', 'Custom sync message')
  event.returnValue = 'sync-reply'
})
```

##### Remote module and IPC
- Remote module allows a process to be executed in renderer process i.e. opening a window.
- This is done synchronously and handled internally
- To implement asynchronous method, we have to use IPC to avoid blocking code scenarios.

### Menus
**[Documentation](https://www.electronjs.org/docs/latest/api/menu)**
- We can create custom menus based on requirements.
- 2 types of menus in elctron
	1. **Application menu**
	2. **Context Menu**

##### Application Menu
- An application menu is the menu appearing on the toolbar
- **Default** application menu
![[def_application_menu.png]]
- **Custom** application menu
![[application_menu.png]]
##### Context Menu
- Context menu pops up when user uses right click.

###### Creating a custom menu
- Similar to window creation, we can create a method and call it when app is ready.
- `Menu` module from `electron` is used to create a menu.
- Attributes are the properties for creatig a custom menu. We create a template using the combination, collection of these attributes.
- **Attributes**:
1. `label`: an attribute to define the label of the menu.
```
const template = [
	{label: 'File'},
	{label: 'Edit'},
	{label: 'View'},
	{label: 'Help'}
]
```
2. `submenu`: an attribute that is a collection of further sub options under a parent option.
```
const template = [
	{
		label: 'File',
		submenu: [
			{label: 'New'},
			{label: 'Open'},
		]
	}
]
```
3. `click`: an attribute that provides functionality to the option
```
const template = [
	{
		label: 'Help',		
		click: function(){
			electron.shell.openExternal('http://electron.atom.io')       // find shell module in next chapter
		}
	}
]
```
4. `type`: an attribute to define the type of a label or empty enitiy. e.g. 
```
const template = [
	{
		label: 'Help',
		type: 'checkbox'
	}
]
```
5. [`accelerator`](ElectronJS#accelerators): an attribute to assign a shortcut to an option in the menu
```
const template = [
	{
		label: 'Help',
		type: 'checkbox',
		accelerator: 'CtrlorCmd + shift + H'
	}
]
```
Output (`label`,`submenu`,`click`,`type`):
![[application_menu_options.png]]

6. **`role`**: General applications have a default menu e.g. edit menu. Hence, electron provides a `role` attribute which allows us to implement a menu with pre-built functionalities. Electron handles these roles on its own. Pre-built functionalities might have already-defined shortcuts. Check **[documentation](https://www.electronjs.org/docs/latest/api/menu)** for various `role` options.
```
const template = [{
	label: 'Edit',
	submenu: [
		{role: 'undo'},
		{role: 'redo'},
		{type: 'separator'},
		{role: 'cut'},
		{role: 'copy'},
		{role: 'paste'},
	]
}]
```

Output(`role`):
![[role_example.png]]

###### Cutom Application menu
- A menu is built from `Menu.buildFromTemplate()` method, which takes one argument `template`.
- A `template` is a collection/array of objects & attributes that define a menu and it's options.
- Return value of `Menu.buildFromTemplate(template)` is used as argument to function `Menu.setApplicationMenu()` method
```
function createAppMenu(){
	const template = [{...}]
	const menu = Menu.buildFromTemplate(template);
	Menu.setApplicationMenu(menu);
}
```
- Finally, call `createAppMenu()` when app is ready

###### Cutom Context menu
- Unlike **application menu**, context menu is created in different manner
- We create an object of class `Menu` and **append** objects of class `MenuItem` from `electron` module
`const {electron, app, Menu, MenuItem} = require('electron');`
```
const ctMenu = new Menu();
```
- `MenuItem` class allows us to initialize a menu item with the provided template
- Argument for `MenuItem` constructor should be a single object containing attributes. Note that the template is not similar to collection of objects like in application menu creation
```
let _copy = {
	label: 'copy',
	click: ()=>{
		// your code
	}
}

ctMenu.append(new MenuItem(_copy))
```
- Next, we add an event listener for the ***right click*** event and suggest custom popup logic
```
win.webContents.on('context-menu',(e,params)=>{
	ctMenu.popup(win, params.x,params.y)
})

// win is the window object we created initially
// params.x and params.y suggest the x,y position of the cursor inside the window
```
**Note**: `Application menu` & `Context menu` APIs in `electron` only work if `frame: true`. These APIs will not work for a frameless window

### Accelerators
- **Accelerators** are the keyboard shortcuts assigned to make app usage faster
- Menus create with `role` attribute do not need shortcuts as they are assigned by `electron` by default
- Accelerators work only when your application is in focus
- If one wants to use shortcuts even when app is out of focus, we assign **Gloabal shortcuts**
```
{
  label: 'Create file',
  accelerator: 'Ctrl + N'
}
{
  label: 'Template store',
  accelerator: 'CmdOrCtrl+shift+T'
}
```
**Note**: for cross-platform usage, use `CmdOrCtrl` instead of only `Ctrl`

### Global shortcuts
- **Global shortcuts** are keyboard shortcuts that work even when application is out of focus.
- We register global shortcuts inside **`main.js`**.
- We need to import `globalShortcut` module from `electron`
`const globalShortcut = require('electron').globalShortcut`
- A global shortcut is always registered when the app is ready and not before that otherwise error will be thrown
- **Registering a global shortcut**
```
app.whenReady().then(function(){
	globalShortcut.register('CmdOrCtrl + 1', ()=>{
		win.show()
	})
})
```
- After exiting, we need to deregister the global shortcuts to avoid issues
```
app.on('will-quit', () => {  
	globalShortcut.unregisterAll() 
})
```

### Shell Module
**[Documentation](https://www.electronjs.org/docs/latest/api/shell)**
-  **shell** module allows to ***open a folder***, ***open a file within a folder***, ***open external url in browser*** 
```
const {shell} = require('electron')
// Create a button in HTML and add following event to it.
```
- **shell** module provides three main (refer documentation for more functionalities) functionalities
1. Select an item in given path
	- Selects and shows a mentioned file in mentioned path
	- If file doesn't exist, no file will be selected
	- If the folder in given path is already open, new window will not be created. Instead, the window will be highlighted
```
btn.addEventListener('click',()=>{
	shell.showItemInFolder(complete_path_to_file)
})
```
2. Open a file in a given path
	- Opens an existing file
	- If file is already open, same file will be opened again unlike the `showItemInFolder` method
```
btn.addEventListener('click',()=>{
	shell.openPath(complete_path_to_file)
})
```
3. Open a link/url in default browser
	- Opens the given url in the default system browser
	- Opens multiple tabs if already open
```
btn.addEventListener('click',()=>{
	shell.openURL(complete_url)
})
```

### Tray Module
**[Documentation](https://www.electronjs.org/docs/latest/api/tray)**
- Add icons and context menus to the system's notification area.
- Implementation is different for different OS (check documentation)
`const Tray = require('electron').Tray`
##### Windows implementation
- initialize a tray object when app is initialized
```
let iconPath =  complete_path
let tray = null;
app.on('ready',()=>{
  tray = new Tray(iconPath);
  ...
}
```
- create a *context menu* and assign it to the `tray` object (inside app)
```
const template = [
    {
      label: 'Audio',
      submenu:[
        {label: 'EN', type: 'radio', checked: true},
        {label: 'MR', type: 'radio', checked: false}
      ]
    },
    {
      label: 'Video',
      submenu:[
        {label: '720p', type: 'radio', checked: true},
        {label: '1080p', type: 'radio', checked: false}
      ]
	}
]

const menu = Menu.buildFromTemplate(template)
tray.setContextMenu(menu);
```
- create a tooltip 
`tray.setToolTip('Tray_Application')`
**Note**: all this code is inside `ready` event of application