# CMM
A compositioning manager manager using deny lists. Automatically starts- or stops a compositioning manager, depending on wether a program from the given deny list is running. An example:

    ./cmm -s 'picom > /dev/null 2>&1' -p 'firefox' -x 'Zoom Webinar' start

Will automatically keep picom running unless it detects that either firefox is running, or a Zoom Webinar is ongoing.

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
Currently CMM can be configured with a config file (`~/.config/cmm.conf`) environment variables or runtime flags (in increasing priority).

| Configuration option | Environment variable | Runtime flag     | Description                                                              |
| -------------------- | -------------------- | ---------------- | ------------------------------------------------------------------------ |
| start                | `CMM_START_COMMAND`  | `-s`, `--start`  | Sets the command used to start the compositioning manager.               |
| kill                 | `CMM_KILL_COMMAND`   | `-k`, `--kill`   | Sets the command used to kill the compositioning manager.                |
| ps-deny-list         | `CMM_PS_DENY_LIST`   | `-p`, `--ps-deny`| Sets the ps-deny-list, which is a comma-separated list of process names. |
| x-deny-list          | `CMM_X_DENY_LIST`    | `-x`, `--x-deny` | Sets the deny-list, which is a comma-separated list of window titles.    |
| poll-delay           | `CMM_POLL_DELAY`     | `-d`, `--poll-delay` | The amount of time (in seconds) between polling.                     |

The start option is mandatory, and at least one deny-list must be given. The kill command by default will use `killall -q` and guesses the compositioning manager's executable name from the start command. The poll delay defaults to 1. Setting this higher would potentially lower CPU usage but this program is quite trivial anyway!

Two possible deny-lists can be defined. The `ps-deny-list` is a comma-separated list of process-names that will be fed to pgrep - if a process match is found, the CM will be killed. The `x-deny-list` is a list of window titles to watch out for. This can be useful for programs that run on the background but only cause issues when they have open windows.
