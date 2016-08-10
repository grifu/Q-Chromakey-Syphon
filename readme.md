# [Qlab Chromakey with QuartzComposer](http://www.virtualmarionette.grifu.com)

## QC Chromakey

Framework for Real-time chromakey in QLAB.

The main goal was to provide live chromakey to camera cues inside Qlab using still images or video files. My work  adapts the solution provided by George Toledo implementing it in QLAB.

![Peregrinacao](http://www.grifu.com/vm/wp-content/uploads/2014/05/s8.jpg)

It was developed by Luís Leite (GRIFU) for the Digital Media PhD with the support of FCT - Fundação para a Ciência e a Tecnologia.

### Q-Chromakey-Syphon
Q-Chromakey-Syphon provides a live chromakey in Qlab using a video or any kind of real-time animation send via a syphon server.

![Qlab compositing](http://www.grifu.com/vm/wp-content/uploads/2014/05/s9.jpg)

You just need to drag the Q-ChromaKey-syphon.qtz into the Video Effects tab of a camera/video cue and define the color to key, adjust the threshold and the smoothing values and finally define the image location (path and filename).

![chroma Syphon](http://www.grifu.com/vm/wp-content/uploads/2014/05/s13.jpg)

The background image is defined by the input “Image Location” in the Image Importer object and will be available in the Qlab interface as a string field. I extract the frame dimensions of the foreground video/camera sent by Qlab using a “Image Dimensions” object witch will adjust the image defined in the Image Importer using a “Image Crop” object. An object called the “Core Image Filter” receives both foreground and background images in separated inputs. This object provides a color type exposed to the exterior (Qlab) that defines the color to key, a threshold and a smoothing input values also exposed to the exterior make the amount of keying interactive from Qlab. This object implements a function script that blends the two images based on the color key (script available above). Finally the result image returns to Qlab.

This framework is a mix between the Q-Chromakey-still.qtz and the Syphon-Movie compositions.
To load and play the background videos you need to send OSC commands from QLAB and you must also run another Quartz composition, the Q-VideoServer.qtz witch solves acts as a syphon server. You can also send any video or animation from other applications like unity or aftereffects.

// Chromakey script adapted from George Toledo
kernel vec4 multiplyEffect(sampler image, sampler backgroundImage, __color color, float threshhold, float smoothing)
{
float Y, Cb, Cr;
float maskY, maskCb, maskCr;
vec4 sourcePixel = sample(image, samplerCoord(image));
vec4 backgroundPixel = sample(backgroundImage, samplerCoord(backgroundImage));
float blendValue;maskY = 0.2989 * color.r + 0.5866 * color.g + 0.1145 * color.b;
maskCr = 0.7132 * (color.r – maskY);
maskCb = 0.5647 * (color.b – maskY);
Y = 0.2989 * sourcePixel.r + 0.5866 * sourcePixel.g + 0.1145 * sourcePixel.b;
Cr = 0.7132 * (sourcePixel.r – Y);
Cb = 0.5647 * (sourcePixel.b – Y);blendValue = smoothstep(threshhold, threshhold + smoothing , abs(Cr – maskCr) + abs(Cb – maskCb));
sourcePixel = sourcePixel * blendValue;return sourcePixel + (backgroundPixel * (1.0 – blendValue));
}



## License

    Copyright 2012-2016 Luís Leite

                    GNU GENERAL PUBLIC LICENSE
                       Version 3, 29 June 2007

 Copyright (C) 2007 Free Software Foundation, Inc. <http://fsf.org/>
 Everyone is permitted to copy and distribute verbatim copies
 of this license document, but changing it is not allowed.

                            Preamble

  The GNU General Public License is a free, copyleft license for
software and other kinds of works.




## Support

![Support](http://www.virtualmarionette.grifu.com)

