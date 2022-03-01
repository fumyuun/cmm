# CMM
A compositioning manager manager using a deny list. Automatically starts- or stops a compositioning manager, depending on wether a program from the given deny list is running. An example:

    ./cmm -s 'picom > /dev/null 2>&1' -d 'firefox' start

Will automatically start picom when firefox is _not_ running, and kill picom when it detects firefox is running.

## Usage
General usage is as follows:

    ./cmm [-h|--help] [options] <action>

Where action is one of the following:

* start:   Starts CMM
* stop:    Stops a running CMM instance
* status:  Prints wether CMM is running or not, and if the managed CM is running.
* inhibit: Manually inhibit the CM from running, which will stop it until released.
* release: Release an inhibited CM, restarting it again.

Only the start action accepts any more parameters to configure it.

## Configuration
Currently CMM can be configured with environment variables or runtime flags (the latter takes priority).

| Environment variable | Runtime flag    | Description                                                           |
| -------------------- | --------------- | --------------------------------------------------------------------- |
| `CMM_START_COMMAND`  | `-s`, `--start` | Sets the command used to start the compositioning manager.            |
| `CMM_KILL_COMMAND`   | `-k`, `--kill`  | Sets the command used to kill the compositioning manager.             |
| `CMM_DENY_LIST`      | `-d`, `--deny`  | Sets the deny-list, which is a comma-separated list of program names. |
|                      | `-t`, `--timer` | The amount of time (in seconds) between checking.                     |

Out of these options, all except the kill command and the timer value are mandatory. The kill command by default will use `killall -q` and guesses the compositioning manager's executable name from the start command. The timer defaults to 1. Setting this higher would potentially lower CPU usage but this program is quite trivial anyway!
