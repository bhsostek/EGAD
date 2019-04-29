# Electron Grid of Aligned Documents

The EGAD framework allows users to quickly create grid based web applications by speicifying a grid size, then populating that grid with prebuilt content. There are many differnet modules built into the framework that help a developer get up and running, such as a Code Editor, File Tree, and a WebGL Canvas. 

[![EGAD Presentation](https://img.youtube.com/vi/BiBUdq4y2fw/0.jpg)](https://www.youtube.com/watch?v=BiBUdq4y2fw)

## Code Editor
<p>The Code Editor widget is a language agnostic editor which allows a user to edit an arbitrary language specified by the developer. This widget also contains a custom Language Parser which suggests commands that a user could be trying to type. Code Mirror  is the base editor tool that the widget is designed and built around. Code Mirror is open source, widely used, incredibly flexible, and well documented making it an excellent choice for a language agnostic editor. The widget takes in an additional parameter called “languageMap” which associates file extensions with languages. This widget has configurable hotkeys for saving the code being edited to a file as well as, copying, pasting, and commenting code. The widget provides a way to register function callbacks to be executed whenever one of these hotkeys is triggered.
</p>
<p>The language parser works by loading a JSON object which describes the language. Information such as variable keywords, scope declaration characters, comment headers and footer as well as any function names are included in this object. When a file is loaded, the file extension is checked against a map of known associations stored within the Code Editor widget. For example, if a file was opened with the extension ‘.js’, the JavaScript configuration file would be loaded. Then all scopes within the file are detected and a tree structure is generated. This tree has information about the line start and end of every scope in the file. This information allows the Language Parser to know exact which scope any line of the file is within. Additionally, whenever a new line is added or removed from the file, all scopes below that point are offset such that the line start and end values of all scopes in the tree hold true throughout editing the file. After all scopes have been established, the file is iterated through line by line and broken up into smaller tokens. These tokens are compared to the list of known variable keywords to try and determine what the user is trying to type. When variables are created, they are inserted into the scope that the cursor currently.
</p>

## File Tree
<p>One feature which many projects rely on is the ability to traverse a file structure. To implement this inside EGAD the FancyTree  library was selected and wrapped within a widget. This library requires the developer to pass a JSON object representing a file directory structure to the fancy tree class constructor. In order to generate this JSON object, the NodeJS file system module was used. This module allows a computers disk to be looked through within JavaScript. This functionality was abstracted within EGAD to the getProjectFiles() which allows a user to get all files and subdirectories of a specific directory. This method also utilizes an ignore object to omit certain files from the JSON object returned. Much like a .gitignore , this object is a blacklist of files to exclude from the returned object. If the developer wanted to ignore all html files in all directories, they would use the wildcard ‘*’ character. This would appear as “*.html” inside of their ignore object. If only a specific file should be ignored that file can be excluded by simply typing its name “index.html” inside the ignore object. If all files of a specific name with different file extensions should be ignored, the wildcard character could be used again. The following would ignore all files named example with any file extension. “example.*”.
</p>
In future versions of EGAD this module will be improved to support remote file server access through the FTP  protocell, however this is out of scope of the first version of the project. This would enable developers to create far more complex applications. Connecting to a remote server may be out of scope for this version of EGAD but that does not mean that it cannot be implemented in the framework currently. An independent developer would be able to create their own FTP widget which could connect to a database and display the resulting files in a tree structure. 

## Canvas Widget
<p>The Canvas widget was designed to provide an easy way for developers to integrate with webGL . WebGL is a web implementation of the Open Graphics Library  (openGL) which provides hardware acceleration for graphics applications on web pages. Since each cell of the EGAD grid is its own web page webGL can be used to provide a hardware accelerated canvas to the developer. WebGL canvases are often used as the basis for HTML5 Games. If a developer were to make a game engine based off the EGAD framework, a cell could be populated with a webGL canvas and other cells could be used for debug information and world editing utilities. Also since EGAD is written on top of Electron the project can be compiled to native executables and distributed across systems easily. 
</p>
<p>The Canvas widget itself preserves no information between runs of the application, and simply acts as a display which other JavaScript files can subscribe to. If a developer writes a piece of code to integrate with webGL, such as a render function that draws an element, they can send this element to the canvas by calling ‘canvas.subscribeToDraw(<drawFunction>)’. This function adds the passed function to an array of callbacks to be executed whenever the canvas redraws. This way developers can add draw functionality to the canvas class without modifying the canvasWidget class itself. These callback functions are executed at a fixed interval, the fastest the canvas can be refreshed is 250 times per second. Every additional draw call added to this function increases the total time it takes to render a frame of the game possibly decreasing the overall performance. Since most games target 60fps, there is lots of leeway to add additional draw calls. If a developer wants to set the canvas to render at a fixed frame rate, they can simply call ‘canvas.setFrameRate(int frames)’ to limit the refresh rate to ‘frames’ per second. </p>
