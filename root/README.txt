The root directory is the directory where all of your resources will live. The file config.json is the file generated by saving the application.
Any persistent data, weather it be configuration data, widget sizes, widget saves, should live in this file. If there are additional resources
that your project needs to load, they should be stored in here. For instance, if you want to load an html file into a webview, you should put
that .html file and all resources needed for that webpage into a folder much like the 'root/documentation' folder.