# justawiki

This is 2019 research compiling various libraries and techniques for automating mouse and keyboard inputs in Python. It covers tools for simulating clicks, detecting key presses, and handling input events across different operating systems. The focus is on solutions that support "while key is pressed" functionality, low-level input handling, and interface scripting.

Key areas explored include:
    Mouse automation (clicking, movement, button presses)
    Keyboard simulation (sending keystrokes, handling key events)
    Hooks and event listeners (detecting key states, implementing "while key is pressed")
    Platform considerations (Linux vs. Windows support, dependency requirements)

This research aims to identify the most effective tools for different use cases, whether for automation, scripting, or game development.

It was originally in the Wiki section because I was interested in learning about GitHub's Wiki functionality at the time.
---


 winner
                    https://nitratine.net/blog/post/python-auto-clicker/
                    https://theembeddedlab.com/tutorials/keylogger-python/

                
    
                    ===mouse (press button)===      
      https://pypi.org/project/pynput/
Looks promising.  Mouse+KB.  Appears to support WHILE KEY PRESSED.
Deps: Xwindows, 

          https://github.com/asweigart/pyautogui
programmatically control the mouse & keyboard. 
Deps: Linux needs the python3-xlib and Pillow

          tkinter?
          https://stackoverflow.com/questions/46227617/tkinter-event-for-downpress-of-mouse-button-holding-down


          https://github.com/autopilot-rs/autopy
Could be good, but is some pseudo-rust thing

          https://github.com/gvalkov/python-evdev/issues/70
A raw solution

      https://github.com/pywinauto/pywinauto
      https://pywinauto.readthedocs.io/en/latest/code/pywinauto.mouse.html?highlight=mouse
      https://stackoverflow.com/questions/25660685/how-to-simulate-keys-in-game-with-pywinauto
Not really for linux
Pywinauto tries to let you interact with the element of a windows application, so also works, if the program is not in the foreground.

                    ===hook for input and WHILE KEY PRESSED===
          https://stackoverflow.com/questions/42104376/how-to-have-python-function-run-only-while-key-is-pressed
pygame...while key pressed, using pygame.  pygame gets mentioned a lot for WHILE PRESSED

          https://github.com/JeffHoogland/pyxhook
Use this to avoid root (keyboard lib req's root), but more of a toggle than a WHILE

          https://stackoverflow.com/questions/52055063/python-pyautogui-working-together-with-pynput
Kinda ghetto, more of a toggle than a WHILE

          good reference on why this is hard to do
          http://blog.robertelder.org/detect-keyup-event-linux-terminal/
                              ===keys===
      https://stackoverflow.com/questions/25660685/how-to-simulate-keys-in-game-with-pywinauto
Comments include notes about DirectX restrictions/interactions
"Sending Virtual Keys will be ignored if the game is running on DirectX. Use sendinput and send the scan_codes instead."

      https://github.com/boppreh/keyboard
      https://stackoverflow.com/questions/44884535/record-while-key-is-pressed-stop-when-key-is-released
Apparently the main/default, but...
"To avoid depending on X, the Linux parts reads raw device files (/dev/input/input*) but this requries root." – jrouquie Aug 6 '18 at 13:39

      https://pyglet.readthedocs.io/en/pyglet-1.3-maintenance/
pyglet is interesting for keyboard and hooking, but doesn't appear to send mousebuttons

      https://bitbucket.org/kitsu/pyahk
unupdated since 2014




      https://www.pygame.org/wiki/about
      http://www.pygame.org/docs/ref/event.html#pygame.event.Event
      https://www.pygame.org/docs/ref/mouse.html
      https://www.reddit.com/r/pygame/comments/6ddbqq/how_do_you_enable_holding_keys_in_pygame/
      https://stackoverflow.com/questions/42104376/how-to-have-python-function-run-only-while-key-is-pressed
Interesting option for *creating* games, not necessarily scripting for them
