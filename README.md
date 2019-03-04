# SKINNYDIP MMU2 string eliminator
a post processing script for Slic3r PE to remove fine threads from filament tips during MMU2 toolchanges.
Written by Erik Bjorgan based on a core concept from David Shealey.
With love to the Prusa Community Forum and its incredible admin team.
http://facebook.com/groups/prusacommunity

# This tool is quite experimental, and probably always will be.  Use it at your own risk! 
Builds tested:
* 1.41.2+linux64
* 1.42.0-alpha7+linux64

While it is possible that it may work with other versions of Slic3r, please do not assume this to be the case, as any changes to the gcode structure expected by this script could lead to gcode that has unintended effects, some of which may be dangerous.   If you choose to use this, understand that you're a guinea pig. 

## Purpose:
This script is used to eliminate the stubborn threads of filament that
can jam the MMU2 during toolchanges  by providing a brief secondary dip
into the melt zone immediately after the cooling moves complete.   

## Installation:
In Slic3r PE, provide the absolute path to the skinnydip.py script in Print Settings > Output Options > Post-processing scripts.   
eg.  /home/username/some_folder/skinnydip.py 
##### Known issue with installation on Linux appimage builds
At the time of this initial release Linux appimage builds of SLic3r PE are misconfigured in a way that prevents running Python scripts directly.  Use skinnydip_appimage_workaround.sh in the post processing script field instead.

## Usage:
The script is configurable via comments made in the filament profile's start gcode section 
Every filament used for this script needs to have the following lines appear at the top of its filament start gcode.

```
M900 K{if printer_notes=~/.*PRINTER_HAS_BOWDEN.*/}200{else}30{endif}; Filament gcode
; SKINNYDIP CONFIGURATION START
; material_type PLA
; material_name my PLA Spool
; insertion_speed 2000 
; extraction_speed 4000
; insertion_pause 0 
; insertion_distance 31 
; removal_pause 0
; SKINNYDIP CONFIGURATION END
```

## Explanation of configuration parameters:
|Parameter          |Explanation                                              |Default Value |
|--------------------|----------------------------------------------------------|---------------|
material_type     | Name of type of material                                    | n/a
material_name     | User defined name for this filament                         | n/a
insertion_speed   | Speed at which the filament enters the melt zone after cooling moves are finished. | 2000 (mm/min)
extraction_speed  | Speed at which the filament leaves the melt zone.  Faster is generally better | 4000 (mm/min)           
insertion_pause   | Time to pause in the melt zone before extracting the filament.| 0 (milliseconds) |
insertion_distance| Distance in mm for filament to be inserted into the melt zone.  This is hardware specific, and shouldn't change very much from one material to the next.  If blobs appear on the wipe tower, this setting is probably too high. | 31 (mm)                       
removal_pause     | Number of milliseconds to pause in the cooling zone prior to extracting filament from hotend.  This pause can be helpful to allow the filament to cool prior to being handled by the bondtech gears. |  0 (milliseconds)   
                  
## Goals:
This method is highly effective for removing fine strings of filament.
This script is intended as a proof of concept, with the hopes that this
functionality would be added to a future revision of Slic3r PE.



