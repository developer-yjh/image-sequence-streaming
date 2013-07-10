image-sequence-streaming
========================

Image sequences are a powerful tool for creating interactive content with outstanding visual quality in iOS applications. Unfortuantly, conventional image sequences (using UIImageView or homebrew) have several severe drawbacks as they require all, or many frames, to be loaded into memory at once.
- Long load times.
- Huge memory footpring.
- Hard limit on the number of frames due to memory.
- Slow framerates due to core graphics/UIKit limitations.

This project aims to overcome these limitations by streaming individual frames, as fast as possible, from an optimized sequence file. This unique approach offers many benefits over traditional methods:
- No load times.
- Constant small memory footprint (a few frames at most) regardless of frame count.  
- Nearly unlimted sequence length.
- No image quality loss.
- Silky smooth framerates that often reach 30-60 FPS which is faster than conventional video playback.
- Sequence stretching - a sequence can be made at a smaller resolution than displayed to improve performance and reduce data size.

The only tradeoffs for this approach is that filesize for a sequence is often significantly larger than the original images and sequences must be precompiled.


A special thanks to LZ4. This is by far the most effecient decompression algorithim out there (based on testing with >6 competing algorithims), and this project would not have been possible without it.

### Included in this project: ###
- A command line tool for building a collection of PNGs into a single optimized sequence file. (sequencebuild)
- A C module for loading sequences and random frame access. (ISSequenceStream)
- An Objective-C wrapper for the C module (ISSequence)
- OpenGL ES and Core Video powered Objective-C view classes for displaying interactive sequences. These include a base renderer, linear playback, horizontal and vertical drag control, and grid cell control, (ISSequenceView, etc)
- A sample project and sequence.


**Note:** Simulator performance is not a good indicator of device performance especially in this project. Always be sure to test on device.


### Usage: ###

```Objective-C
/* loading a sequence */
ISSequence* sequence = [ISSequence sequenceNamed:@"sequence.seq"];

```

```Objective-C
/* creating a drag control view */
ISSequenceDragView* view = [[ISSequenceDragView alloc] initWithSequence:sequence
                                                        refreshInterval:1 /* refresh rate */
                                                        useTextureCache:YES /* texture cache is an optional core video optimization */
                                                                  loops:true
                                                                  range:[sequence range]
                                                          dragDirection:kISSequnceDragDirectionHorizontal
                                                        dragSensitivity:2.0
                                                               delegate:NULL];
															   
[self addSubview:view];
															   
```

```Objective-C
/* creating a grid control view */

/* 
 when touch is enabled for grid view touches cause the view to load the grid cell nearest to the touch
 */

ISSequenceGridView* view = [[ISSequenceGridView alloc] initWithSequence:sequence
                                                        refreshInterval:1
                                                        useTextureCache:YES
                                                                  range:[sequence range]
                                                           framesPerRow:21
                                                           touchEnabled:YES];

															   
[self addSubview:view];
															   
```


