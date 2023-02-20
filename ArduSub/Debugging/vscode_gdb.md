Install cpp-tools
Install gdb

launch.json:

```
// Common ArduPilot Debugging Profiles for VSCode
// see https://ardupilot.org/dev/docs/debugging-with-gdb-using-vscode.html
//
// GDB must be installed!
//  To install GDB on a Debian based system: `sudo apt install gdb`
//
// Be sure that SITL or WAF have been set to generate debugging symbols
//     sim_vehicle.py : use '-D', `./Tools/autotest/sim_vehicle.py -v Copter -D --speedup 1 --console --map`
//     waf            : use `--debug`, `./waf configure --board=sitl --debug`
//
// The examples below are given for plane and copter vehicle types. To create profiles for other vehicles
// change the "name" field & "program" field to the desired vehicle

{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [

    // The attach profiles are used to connect to a running instance of SITL (ie the executable)
        {
            "name": "(gdb) Attach Sub",
            "type": "cppdbg",
            "request": "attach",
            "program": "${workspaceFolder}/build/sitl/bin/ardusub",
            "processId": "${command:pickProcess}",
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        },
    // The `Launch` profiles allow debugging of the binaries directly for initialization tasks and constructors, etc. without using MAVProxy.
    // Note: The easiest method to debug initialization and constructors is to select the debug points, attach to the executable, and then command "reboot" in MAVProxy.
    // Launch tasks require that the binary to be debugged has already been built. Be sure to run ``./waf copter`` beforehand or the old binary will be debugged instead.
        {
            "name": "Launch ArduSub (Debug)",
            "type": "cppdbg",
            "request": "launch",
            "cwd": "${workspaceFolder}/ArduSub",
            "program": "${workspaceFolder}/build/sitl/bin/ardusub",
            "args": [
                "-S",                                // Wipes simulated eeprom to defaults
                "--model", "vectored_6dof",                        // Set the how much faster relative to real-time the simulation runs
                // "--uartE=sim:lightwareserial",        // Used to attach simulated serial devices
                "--speedup","1",
                "--slave", "0",
                "--defaults", "${workspaceFolder}/Tools/autotest/default_params/sub-6dof.parm",
                "--sim-address=127.0.0.1",
                "--home", "33.810313,-118.39386700000001,0.0,270.0",
                "-I0"
                // "--help"                              // Lists all commands available
            ],
            // "stopAtEntry": false,
            // "environment": [],
            // "externalConsole": false,
            "miDebuggerPath": "/usr/bin/gdb",
            "MIMode": "gdb",
            "launchCompleteCommand": "exec-run",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}
```
