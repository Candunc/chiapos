# Chia Plotter JSON API

Version: 0.0

Updated: May 29, 2021

## Introduction

Manually reading the log files of the Chia Proof of Space Plotter is acceptable for single machines, but as the
 ecosystem grows tools will be developed to distribute the generation of plots across multiple machines.
Creating a unified data format will allow developers easy access into the status of the plotter, and paves the path
 for a better experience for the end user.

## Formatting

Each status update generated by the plotter will consist of a valid JSON object, followed by a linebreak. There
 are two root level entries: "data", and "error".
They are mutually exclusive, with the "error" object corresponding to an object with the text literal from
 plotter_disk.hpp.
The data entry is strongly typed from the first level, containing the following entries:
| entry  |      type       | description                                                    |
| :----- | :-------------: | :------------------------------------------------------------- |
| phase  |     Integer     | corresponding to the four phases of plotting                   |
| bucket |     Integer     | indicating the sub-entry of that phase.                        |
| format |     Integer     | Specifies the formatting of the info object                    |
| info   |     Object      | containing information specific to that phase.                 |
| time   | ISO 8601 String | Contains the date and time of when the JSON object was created |

The info object varies from phase-to-phase, and has six different forms:

#### Phase 0: (format == 0)

| entry       |  type   | description                                                      |
| :---------- | :-----: | :--------------------------------------------------------------- |
| id          | String  | Contains the ID of the plot that is being generated.             |
| plot_size   | Integer | The k value of the plot.                                         |
| buffer_size | Integer | The requested maximum RAM consumption in MiB.                    |
| buckets     | Integer | The number of buckets being used to generate the plot.           |
| dir_tmp     | String  | The first temporary directory                                    |
| dir_tmp2    | String  | The second temporary directory                                   |
| dir_final   | String  | The final directory, usually the plotting directory of a farmer. |

#### Phase 1: (format == 1)
