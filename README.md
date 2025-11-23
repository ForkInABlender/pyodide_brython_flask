# What is in this repository?

Flask, pyodide.js version 0.25.1, brython.js version 3.11.3, and webworkers+websockets, both frontend and backend.

Thus far it is a basic template. Later it will be adapted to a much larger framework.


in the meantime, enjoy this basic ascii art graph to conceptualize the points that connect:

```
        +------------------------+-----------------------------------------------+----------------------------------+
        | Browser (Main Thread)  |         Browser (Web Worker 'w')              |          Flask Backend           |
        |  (index2.html/Brython) |      (index2.html/Brython)                    |            (app.py)              |
        +------------------------+-----------------------------------------------+----------------------------------+
        |                                                                        |                                  |
        | 1. Create Worker                                                       |                                  |
        |----------------------->                                                |                                  |
        | 2. Worker Ready Signal                                                 |                                  |
        | (Sends {'msg': 'Main.ready'})                                          |                                  |
        |----------------------->|                                               |                                  |
        |                        | 3. Worker Processes                           |                                  |
        |                        |  (Sends {'msg': 'Main.ready', 'worker': 'w'}) |                                  |
        | <----------------------|                                               |                                  |
        | 4. Main Thread Handles |                                               |                                  |
        |                        |                                               |                                  |
        | 5. Send over WebSocket |                                               |                                  |
        | (ws.send("Main.onmsg<--Worker-w: ..."))                                |                                  |
        |----------------------------------------------------------------------->|                                  |
        |                        |                                               |                                  |
        |                        |                                               | 6. Backend Receives              |
        |                        |                                               |                                  |
        |                        |                                               | 7. Backend Echoes                |
        |                        |                                               | (ws.send("/ws returned: ..."))   |
        | <----------------------------------------------------------------------|----------------------------------+
        | 8. Main Thread Receives|                                               |
        |  (Prints 'Server→Main: /ws returned: ...')                             |
        |                        |                                               |
        V                        V                                               V
```
