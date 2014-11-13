# Optimization Guide #

All games eventually run into needing to optimize the game before publishing the final version.

This document serves to give the reader ideas on where to start on the optimization process.

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

6 - Run the [PerfHUD ES](http://error454.com/2013/11/14/profiling-ouya-tegra-3-games-using-nvidia-perfhud-es/) profiler to measure draw calls and inspect performance.

## Share ##

Be sure to share your optimization ideas so we can improve this guide.
