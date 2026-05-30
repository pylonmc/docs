!!! abstract "Author: Idra"

## Fluid points

A fluid point is one of the red/green/gray cubes you have to click to place pipes. There are 3 types of fluid points: `INPUT` (green), `OUTPUT` (red), and `CONNECTOR` (gray). Input and output points are attached to blocks to allow fluids to be added/removed, while connected are solely for connecting other points together.

To create a fluid connection point for your machine, you can call `FluidPointInteraction.make(...)`.

## Fluid blocks

### PylonFluidBlock

PylonFluidBlock must be implemented by any block that has fluid inputs/outputs. This interface allowed you to request fluids from input points, supply fluids to output points, and specify how to add/remove fluids from your block. Confused? See the 'ticking' section for an explanation of how this works.

See PylonFluidBlock Javadocs for usage information.

### PylonFluidBufferBlock

Usually, fluid machines store fluids in internal fluids. For example, the press has an internal buffer used to store plant oil, of size 1000mB by default. This is a common enough thing that we created a new interface to handle it: PylonFluidBufferBlock. This interface allows your block to easily manage fluid buffers.

See PylonFluidBufferBlock Javadocs for usage information.

### PylonFluidTank

Another common pattern is a 'fluid tank' which can only store one fluid at a time, but can store many types of fluids. PylonFluidTank implements this pattern.

See PylonFluidTank Javadocs for usage information.

## Physical vs virtual fluid points

There are physical and virtual fluid points. 'Physical' fluid points (the displays and interactions used to display the boxes and allow you to interact with them) contain a 'virtual' fluid point. This distinction may seem confusing, but helps keep the underlying logic simple. 

Physical fluid points are mainly just a wrapper around virtual fluid points which provide logic for displaying them and for handling player interactions with them - they aren't too important and you'll almost never need to think about them, so when we say 'fluid point', we are referring to virtual fluid points. 

Virtual fluid points have a few properties, including the UUID of the fluid point (used to reference the point easily), the type of the fluid point, the connected fluid points, and the fluid point's block. If the fluid point is an input or output, its block must be a PylonFluidBlock.

## Segments

Fluids are managed with the help of 'segments'. A segment is just a collection of connected fluid points. Segments encapsulate **all** of the connected fluids points. For example, here (with the dashes indicating pipes), A, B, and C are all in one segment, D and E are in another segment, and F is in yet another segment:
```
A---B-----C  D--E    F
```

If we removed the pipe between B and C, then A and B would form a new segment, and C would also form a new segment. The old segment would be deleted. Conversely, if we connected C and D, then the A/B/C and D/E segments would be merged into one new segments.

## Ticking

Segments are important because they are the basis of how fluid ticking works. Each segment is ticked every few in-game ticks (depending on your server's config). When a fluid segment is ticked, the following happens:

1. We call `getRequestedFluids` for each block in the segment and make a list of what fluids are being requested and how much of each one is being requested.
2. We iterate through every fluid being supplied.
3. We skip the fluid if it's not allowed by the segment.
4. We skip the fluid if it's not being requested by a machine (ie, `fluidAmountRequested` returns zero for every machine).
5. When/if we find a fluid that is being requested by at least one machine, we use a round-robin algorithm to take as much fluid from each machine as we can, depending on how much fluid can be passed through the pipe and how much is being requested.
6. We do another round-robin pass, but this time on the machines that we're sending the fluids to, in order to distribute the fluid between them evenly.

## Fluid flow rates and allowed fluids

We generally want to limit how quickly pipes can transfer fluids. Properly doing this is difficult and computationally expensive, so we just 'lie' about it and instead limit the fluid flow rate **per segment**. We also might want to limit which fluids can pass through a pipe (for example, no hot fluids allowed in wooden pipes). This is again done on a per-segment basis.

This is partially why you cannot connect different types of pipes together: which pipe's fluid flow rate would we use for the segment?

When pipe segments are merged, one of the segments is chosen arbitrarily and its fluid flow rate and allowed fluids is preserved. When pipe segments are split, the fluid flow rate and allowed fluids from the old segment is preserved to the new segments.

## Pipe placement logic

Not even going to try to explain this beast. If you need to touch this code, good luck.

