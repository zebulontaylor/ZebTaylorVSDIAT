## Day 1

Begin by entering the environment (on my machine, via `nix develop`), and run `./flow.tcl -interactive`.

Now you are in OpenLane. Setup the design by running `package require openlane` and `prep -design picorv32a`.

![alt text](image.png)

The first three steps of the OpenLane flow: `run_synthesis, run_floorplan, run_placement`.

![alt text](image-1.png)

To view the output of the floorplan, you can use `magic`. In the tcl shell, run `magic -T $env(PDK_ROOT)/$env(PDK)/libs.tech/magic/$env(PDK).tech lef read $env(RUN_DIR)/tmp/merged.nom.lef def read $env(RUN_DIR)/results/floorplan/picorv32a.def &`.

**For some/most people, the file in the last command will be called `picorv32.def`, not `picorv32a.def`. Update the command accordingly.**


![alt text](image-4.png)

If you do not see the pink/blue lines, you have the wrong PDK path. Make sure you are using the correct relative location and that you spelled everything right, or just use the command I provided.

---

To view the output of the placement, simply modify the folder used in the `magic` command. Run  `magic -T $env(PDK_ROOT)/$env(PDK)/libs.tech/magic/$env(PDK).tech lef read $env(RUN_DIR)/tmp/merged.nom.lef def read $env(RUN_DIR)/results/placement/picorv32a.def &`.

**For some/most people, the file in the last command will be called `picorv32.def`, not `picorv32a.def`. Update the command accordingly.**

![alt text](image-3.png)

To examine the placement, zoom in and out with `z` and `shift+z`, respectively. Navigate laterally with arrow keys. To enter magic commands, type a `:` and then the command, then press enter. You will see what you type appear in the console below. To select an area, left click for one corner and then right click to set the other. Left clicking elsewhere will move your selection box.

Try out commands by typing `:expand` on a portion of the placement.

![alt text](image-2.png)

Expanded

![alt text](image-5.png)

Zoomed in

## Day 2

### Theory

$\text{utilization} = \frac{\text{netlist area}}{\text{core area}}$

The die is a slightly larger area of silicon that surrounds the core, essentially acting as a margin.

In order to more effecively place the combinational logic, logic is broken down into "blocks", which then get placed and layed out by the floorplan. These blocks will later be further detailed by other tools.

As the blocks are placed, they get surrounded with "decoupling capacitors," to stabilize the current draw that occurs during switching inside the blocks (CMOS gates only draw current when switching). This is to prevent voltage drops which would send the voltage of the digital ones into an "undefined" state, resulting in unintended or unknown behavior.

In order to mitigate "ground bounces" or "voltage drops" (when gnd increases or vdd drops), power is distributed in the chip through grided distribution wires with contacts at the intersections.

### Software

To modify the configuration of a design, alter `designs/[yourdesign]/config.tcl`. This does NOT have precedent over the PDK's `.tcl` file, however.

