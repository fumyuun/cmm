# CMM
A compositioning manager manager using a deny list. Automatically starts- or stops a compositioning manager, depending on wether a program from the given deny list is running. An example:

    ./cmm -s 'picom -b' -d 'firefox'

Will automatically start picom with the -b flag when firefox is _not_ running, and kill picom when it detects firefox is running.
