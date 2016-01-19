# Optimization Guide #

All games eventually run into needing to optimize the game before publishing the final version.

This document serves to give the reader ideas on where to start on the optimization process.

## Snapdragon Profiler ##

The Qualcomm developer portal has the [Snapdragon Profiler](https://developer.qualcomm.com/software/snapdragon-profiler) for finding performance bottlenecks and troubleshooting issues on Snapdragon devices like the `Forge TV`. The profiler runs on Linux, Mac, and Windows and connects to `ADB` devices. Online discussion of the profiler is available at the [Snapdragon profiler forums](https://developer.qualcomm.com/forums/software/snapdragon-profiler).

## NVIDIA PerfHUD ES ##

[PerfHUD ES](https://developer.nvidia.com/nvidia-perfhud-es) is a profiler that lets you inspect individual draw calls to find bottlenecks and performance leaks.
Get started fast with the [quick-start guide](http://docs.nvidia.com/gameworks/index.html#developertools/mobile/perfhud_es/perfhud_quickstart_guide.htm). 
Follow the [general overview](http://error454.com/2013/11/14/profiling-ouya-tegra-3-games-using-nvidia-perfhud-es/) for profiling `Cortex` games on the `OUYA` console.

## Steps ##

1 - Optimize shaders. In some cases things calculated in the fragment section can be calculated in the vertex section.

2 - This video shows how to combine materials to a single draw call. In some cases fewer draw calls can have better performance:

<table border=1>

 <tr>
 <td>Reduce Draw Calls (00:12:01)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=O3dbE2t8lPQ" target="_blank">
<img src="http://img.youtube.com/vi/O3dbE2t8lPQ/0.jpg" alt="Quick Tip: Combining multiple materials into a single draw call " width="240" height="180" border="10" /></a>
 </td>
  <td></td>
 </tr>

</table>

3 - The player settings / camera skybox has 6 sides which by default are always drawing. For a platformer only 1-3 sides are visible at any time. You'll get a performance improvement manually drawing your own skybox and set the camera to clear color instead.

4 - Drawing transparent pixels is slow. Be sure to crop the pixels in sprite art to minimize the amount of transparent pixels that are drawn.

5 - Manual culling. Don't just let the GPU handling culling for you. Your scripts can check if an object is seen by the camera and if you manually disable GameObjects you can get back the performance they may be eating. Also you can batch gameobjects in the same vicinity and toggle groups on and off as a sort of super-frustrum.

6 - Run the `NVPerfHUD` profiler to measure draw calls and inspect performance.

* Note: Be sure that the `android.permission.INTERNET` permission is enabled in the `AndroidManifest.xml` to ensure the profiler can be used.

* Note: Enable the `NVPerfHUD` profiler on the `Cortex` device.

```
adb shell setprop debug.perfhudes 1
```


## Share ##

Be sure to share your optimization ideas so we can improve this guide.
