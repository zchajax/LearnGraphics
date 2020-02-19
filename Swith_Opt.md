# Geometry Optimization

- smaller frame buffer (r.ScreenPercentage 20)
- Small ShadowMap (r.Shadow.MaxResolution 128)

# Depth Pre-Pass
- Full Depth-prepass : r.EarlyZPass 2, r.EarlyZPassMovable 1
 - Partial (Main occluders) : r.EarlyZPass 1
 - No prepass : r.EarlyZPass 0, r.SelectiveBasePassOutputs = False

 # LOD
 - r.ViewDistanceScale, r.StaticMeshLODDistanceScale, r.SkeletalMeshLODRadiusScale
 - r.ForceLOD 0

# Foliage

- Turn off grass rendering : foliage.MaxTrianglesToRneder 0 
## Density scaling
- foliage.DensityScale
- foliage.MinimumScreenSize
- foliage.MaxTrianglesToRender

# Post-Processes

- SSR : r.SSR.Quality
- Probe Reflection : r.ReflectionEnvironment
- SSAO : r.AmbientOcclusionLevels
- Depth of Filed(Gaussian) : r.DepthOfFieldQuality
- Motion Blur : r.MotionBlurQuality
- Bloom : r.BloomQuality
- Anti-Aliasing : r.PostProcessAAQuality

# Environment Reflection

- Half resolution version : r.HalfResReflections 1

# DOF 

## 3 types of DOF

- Guassian : fastest, but low visual quality
- BokehDOF : custom sprite, visually nice and accurate
- CircleDOF : ok only at low radius, noisy at large radius

# SSAO

 - Half resolution : r.AmbientOcclusion.HalfRes 1

# Cpu Cores

1. switch has 4 cores, 3 cores can be used, 1 core used by os.

2. UE4 thread distribution

    - Core0 :  Worker Threads, RHI Thread, input Thread, Input Thread, Audio Thread, Audio Thread, IO Thread...

    - Core1 : **Game Thread** , offloads tasks to core0 workers

     - Core2 : **Render Thread**, can rely on RHI Thread offloads tasks to core0 workders

# Cpu Optimization

## Render Thread slow

 - Check RHI thread state: r.thithread.enable
 - reduce number of meshes
 - merge several static meshes into a single single one

## Game thread is slow

- too many actors ticking
- complex bounding box update (capsule is faster than skeletal animation volume update)
- critical code in Blueprint
- if the rendering is light, disable RHI thread to give more cycles to Game thread

## Figure out which BP is heavy or which actors are ticking

 - stat DumpFrame -ms = 0.1 will print infomation in the console.

# GC

## Measure GC pause timing
- in test config, make sure Log is enabled
- log loggarbage verbose
- gc.CollectGarbageEveryFrame 1

## Reducing GC Pauses

 - Reduce number of UObjects(merging actors...)
 - Make sure GC is multi-threaded
 - Call GC less often.

# Memory

- Memory buget: about 3 GB, shared between CPU and GPU.(some devkits have 6 GB)
- Check memory usage: memreport - log -full
- if  "Peak Memory" higher than 2.7GB, may cause crash.

## Memory leak

- tool: MemoryProfiler2.