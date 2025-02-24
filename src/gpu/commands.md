# GPU Commands
GPU commands are composed of a sequence of 32 bit values (which i'll call _packets_) sent through 
registers `GP0` and `GP1`, with the first register being used for rendering commands while the 
latter is used for display commands.

Most commands require a single packet, but some require more.
