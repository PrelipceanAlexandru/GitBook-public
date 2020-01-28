# How to add custom program to Favorites in Ubuntu 18.04

[How to add custom program to Favorites in Ubuntu 18.04](https://www.youtube.com/watch?v=BwBjjkBM_L4)

Open `.local`

```
cd .local/share/applications
```

Create a new file like this:

Replace `NameOfProgram` with your program name
```
nano NameOfProgram.desktop 
```
and add the following:
```
[Desktop Entry]
Comment=NameOfProgram
Terminal=false
Name=NameOfProgram
Exec=/path/to/your/version/NameOfProgram/nameofprogram
Type=Application
Icon=/path/to/your/version/NameOfProgram/icon.xpm
StartupWMClass=NameOfProgram
```

**This is an example using PostmanCanary**

```
[Desktop Entry]
Comment=PostmanCanary
Terminal=false
Name=PostmanCanary
Exec=/home/alex/PostmanCanary/PostmanCanary
Type=Application
Icon=/home/alex/PostmanCanary/canary.svg
StartupWMClass=PostmanCanary
```

Then search in `Show applications` and search for `NameOfProgram`

