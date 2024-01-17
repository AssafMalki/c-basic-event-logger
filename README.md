# c-basic-event-logger

**NOTE: This project was primarily developed for educational purposes and is not intended for use in production environments. For further details, see the [Caveats](#caveats) section.**

This is a straightforward and user-friendly logger designed for logging messages of various levels with timestamps, outputting to both the console and a specified log file.

![Example Screenshot](http://i.imgur.com/1aMpk6I.png)

## Features

* Outputs are timestamped with the date and time.
* Console outputs are color-coded based on log message severity.
* Error levels are designed to automatically display the value of errno, if set.
* Supports variable argument lists.
* Capable of generating and "prettifying" stack traces for improved readability.
* Offers an option to automatically wrap messages exceeding 80 characters.
* Crafted in C for seamless integration with C and C++ projects.

## Caveats
* This logger is not thread-safe.
* It is not safe for use in signal handling.
* Timestamps do not currently support millisecond precision.

## Integrating c-basic-event-logger into Your Project

Start by compiling the `libcbeventlogger.a` library:
```shell
make
```
This command will configure and run CMake in a `build` subdirectory and subsequently run the generated makefile(s) there. The resulting `libcbeventlogger.a` will be moved to the root directory of c-basic-event-logger.

When compiling your project, include the c-basic-event-logger directory in your build path:
```shell
-I <path-to-c-basic-event-logger-dir>
```

Link against `libcbeventlogger.a`:
```shell
-L <path-to-c-basic-event-logger-dir> -lcbeventlogger
```

Then, simply include `cbeventlogger.h` in your project files:
```c
#include <cbeventlogger.h>
```

You're all set! For usage instructions, refer to the section below.

## Usage

Every log message must be assigned a level. The supported levels are:
```c
CBELOG_FATAL   = -2 : A critical error has occurred, resulting in immediate program termination.
CBELOG_ERROR   = -1 : An error has occurred, which may not necessarily cause the program to terminate.
CBELOG_INFO    = 0  : Essential information regarding program operations.
CBELOG_WARN    = 1  : Warnings about situations that may not impact normal operations.
CBELOG_DEBUG   = 2  : Standard debug messages.
CBELOG_VERBOSE = 3  : Detailed debug messages.
```

Here's an example of logging a message:
```c
cbeventlogger.writeLog( <desired_level>, "Message", <var-args> );
```

For instance, to mimic the output shown in the example screenshot:
```c
cbeventlogger.writeLog( CBELOG_FATAL, "Test Fatal" );
cbeventlogger.writeLog( CBELOG_ERROR, "Test Error" );
cbeventlogger.writeLog( CBELOG_INFO, "Test Info" );
cbeventlogger.writeLog( CBELOG_WARN, "Test Warn" );
cbeventlogger.writeLog( CBELOG_DEBUG, "Test Debug" );
cbeventlogger.writeLog( CBELOG_VERBOSE, "Test Verbose" );
```

To log a stack trace, use:
```c
cbeventlogger.writeStackTrace();
```
Stack traces are automatically "prettified" for enhanced readability.

*NOTE: Prettified stack traces are currently not functional on MacOS and will default to standard backtrace output.*

If you wish to load a config file to configure the logger:
```c
cbeventlogger.loadConfig( <desired-config-file> );
```
The config file can include any of the following settings:
```shell
silent=<true-or-false>    // Activates or deactivates silent mode
wrap=<true-or-false>      // Determines if long lines should be wrapped
flush=<true-or-false>     // Decides whether the log file should be flushed
debug=<0-to-3>            // Sets the debug level, from 0 to 3
logfile=<logfile-path>    // Specifies the path for the log file
```
Individual functions are also available to manually adjust each parameter. Refer to the documentation below for more details.

## Documentation

The documentation has been adapted to reflect the new function names and library structure. For detailed information on each function and how to use them, please refer to the updated documentation within the c-basic-event-logger repository.

## Promotion

If you find c-basic-event-logger useful, consider starring it on GitHub and sharing it with others!