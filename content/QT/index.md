---
    title: QT
---
[[index|Home]]

>In Qt, when you attempt to open a resource file (indicated by the `:/` prefix), you cannot open it for writing (`QIODevice::ReadWrite`). Resource files are typically compiled into your application's binary and are read-only by default. Therefore, attempting to open a resource file for writing will fail, resulting in the error you encountered.

>To set equal portions in Layout :
- Create a **QHBoxLayout** container.
- Add your two widgets (let’s call them `widget1` and `widget2`) to this layout.- Set the **stretch factor** for both widgets to 1 (equal parts).
- The widgets will automatically share the available horizontal space equally.


# How to deploy QT your project on windows

- open Qt 6.7.0 (MinGW 11.2.0 64-bit) application 
- cd then full project release file path 
- Run `   windeployqt.exe --quick .   ` command .
- DONE .

YOUTUBE for full setup : [How to Create Executable & Installer (.Exe) File for QT Project (youtube.com)](https://www.youtube.com/watch?v=hCXAgB6y8eA)
