---
title: Log Muncher
parent: Tools
grand_parent: Home
---

# Log Muncher

Log Muncher is a command-line tool that parses BepinEx logs and helps identify which errors are the most in need of attention. At the most extremes, Log Muncher can take a file with over 400,000 lines and condense it into fewer than 100 potential issues, of which, the top 10 will likely contain the most valuable information for debugging.

Log Muncher will output any log it analyzes into easy-to-read html format that can be displayed directly in your browser.

## Table of Contents

- [Log Muncher](#log-muncher)
  - [Table of Contents](#table-of-contents)
  - [Download](#download)
  - [Usage and Arguments](#usage-and-arguments)
  - [LC Problem Matcher](#lc-problem-matcher)
    - [Ignorable Non-Issues](#ignorable-non-issues)
      - [LCM 00000](#lcm-00000)
      - [LCM 00001](#lcm-00001)
    - [Slight Issues](#slight-issues)
    - [Moderate Issues](#moderate-issues)
      - [LCM 20000](#lcm-20000)
      - [LCM 20001](#lcm-20001)
      - [LCM 20002](#lcm-20002)
    - [Serious Issues](#serious-issues)
      - [LCM 35000](#lcm-35000)
    - [Critical Issues](#critical-issues)
      - [LCM 40000](#lcm-40000)

## Download

Currently, Log Muncher is available at [The Community Repo](https://github.com/LethalCompanyModding/LogMuncher) but will require building manually.

## Usage and Arguments

Log Muncher must be sent arguments to tell it what to do. I'll briefly describe how to use each one, below:

<dl>
  <dt>-i</dt>
  <dd>
  
  This tells Log Muncher to use a specific file as input. Optionally, you should indicate an output file with <strong>-o</strong> or Log Muncher will default to the input filename with the extension changed to <strong>html</strong>

  Example usage:
  <pre>LogMuncher -i LogInput.log</pre>

  </dd>

  <dt>-o</dt>
  <dd>
  
  This tells Log Muncher to use a specific file as output. You should indicate an input file as well with <strong>-i</strong> or Log Muncher will not run with only this argument.

  It is strongly recommended to use the extension <strong>html</strong> on output files as that is the format Log Muncher will output to

  Example usage:
  <pre>LogMuncher -i LogInput.log -o Output.html</pre>
  
  </dd>

  <dt>-f</dt>
  <dd>
  
  This tells Log Muncher to use a specific folder as input. All other input and output options will be ignored if this option is set. Log Muncher will output the converted files in a folder underneath the given folder called "munched" and will erase any existing html files with the same name as outputted files.

  Example usage:
  <pre>LogMuncher -f MyFolderFullOfLogs</pre>

  </dd>
  <dt>--quiet</dt>
  <dd>
  
  If this flag appears anywhere in the arguments Log Muncher will not output anything but critical errors to the console window at all. This option causes Log Muncher to run marginally faster

  </dd>
  <dt>--version</dt>
  <dd>
  
  Simply outputs the version number of the program. I often forget to update this, sorry :[

  </dd>
  <dt>--help</dt>
  <dd>
  
  Outputs a basic description of all the flags and inputs Log Muncher accepts. Useful if I forget to update this page when adding new features!

  </dd>
</dl>

For a reminder of these options simply use `LogMuncher --help` at any time.

## LC Problem Matcher

The LC Problem Matcher is the source of Log Muncher's amazing Log Munching ability. Each entry under a log issue will have a problem matcher code in the format of `LCMXXXX` with a short description of the problem as well as a link to this document for the full breakdown

### Ignorable Non-Issues

Errors in this category can be safely ignored so all lines matching them are strongly reduced in weight.

#### LCM 00000

`This warning is completely safe to ignore`

This rule matches when BepinEx notices a mod is compiled against a newer version of BepinEx than the running version. The Lethal Company Community currently uses the 2200 build which is a single revision behind the main build as is completely compatible.

#### LCM 00001

`This error is completely safe to ignore`

This rule matches a common error that is displayed when a player ragdoll is not defined. This tends to be the case for all lobby expander mods but is completely harmless.

### Slight Issues

None Yet

### Moderate Issues

Rules in this category may cause issues but are not guaranteed to do so. As such, lines matching these rules have their weights moderately increased.

#### LCM 20000

`Expression contains a null reference`

This rule matches the classic "object not set to an instance of" output that is a common error.

Null References mean that somewhere along the way an object was lost or never properly set. This tends to be a fairly bad thing as all code operating on that lost object will simply fail. This could mean anything from quietly failing to change a scrap's value to exactly 100 (non-destructive) to a mod losing track of it's own UI interface (very bad).

#### LCM 20001

`BepinEx is skipping a plugin because it already loaded it once`

This rule matches lines that indicate BepinEx has encountered a mod's unique GUID more than once in the chainloader

This is a pretty bad thing, the only way this log can show up is if you have multiple of the same mod installed in different directories under /plugins/ or if two mods are using the same GUID. A GUID is _unique_ by design and mods should not share it under the majority of circumstances and multiple mods using the same GUID should not exist simultaneously.

#### LCM 20002

`A file failed to load due to a sharing violation`

This rule specifically matches _sharing violations_ that indicate a mod was unable to open a specific file (usually an asset bundle). Mods failing to load a file can range in severity from simply using default configurations instead of a loaded config file all the way to becoming completely inoperable due to failing to load an asset bundle.

### Serious Issues

Serious issues indicate something really went wrong but may still be recoverable. As such all lines matching these rules are strongly increased in weight.

#### LCM 35000

`Expression originates from HarmonyX, this is usually a bad sign`

This rule matches errors specifically from HarmonyX. Harmony is the backbone of many mods so this indicates something pretty severe has gone wrong.

### Critical Issues

Critical issues indicate something really went wrong and may not be recoverable. As such all lines matching these rules are drastically increased in weight.

#### LCM 40000

`Expression contains an Exception`

This rule matches when the line contains any valid Exception type following the convention of ending an exception with the word 'exception'. Exceptions tend to mean something went really wrong.
