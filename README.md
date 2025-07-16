Begin by entering the environment (on my machine, via `nix develop`), and run `./flow.tcl -interactive`.

Now you are in OpenLane. Setup the design by running `package require openlane` and `prep -design picorv32a`.

![alt text](image.png)

The first three steps of the OpenLane flow: `run_synthesis, run_floorplan, run_placement`.

![alt text](image-1.png)

To view the output of the floorplan, you can use `magic`. To make it easier, run `export RUN=[run output folder]`, and then `magic -T pdks/sky130A/libs.tech/magic/sky130A.tech lef read $RUN/tmp/merged.nom.lef def read $RUN/results/floorplan/picor*.def &`.

![alt text](image-2.png)

To view the output of the placement, simply modify the folder used in the `magic` command. Run `magic -T pdks/sky130A/libs.tech/magic/sky130A.tech lef read $RUN/tmp/merged.nom.lef def read $RUN/results/placement/picor*.def &`

![alt text](image-3.png)


