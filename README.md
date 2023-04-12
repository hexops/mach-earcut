# mach/earcut: industrial-strength polygon triangulation

Turning polygons into triangle meshes is a challenging problem, with numerous edge-cases. Popular libraries that try to solve this problem include libtess2, libtess3, and poly2tri (including the MetricPanda poly2tri variant.) Some of these libraries are better than others, but all of them suffer from performance and correctness issues-often failing on some very simple polygon inputs.

State-of-the-art research into polygon tesselation includes [CMU researcher Jonathan Shewchuk's outstanding 'Triangle' library](https://www.cs.cmu.edu/~quake/triangle.html), which is probably the most industrial-strength and correct polygon tesselator in existence today. Despite widespread adoption in some open source projects, [it is proprietary](https://gist.github.com/slimsag/7e38961c7f9dfc2dcf8eea17b41f919e) and not legally suitable for inclusion in open source software.

The second most industrial-strength tesselation library in existence today is from [a company called Mapbox](https://github.com/mapbox/earcut), and is [at the core of their map rendering](https://docs.mapbox.com/mapbox-gl-js/example/) technology. `mach/earcut` is a port of their library to Zig. It is open-source and permissively licensed, and based on ideas from [FIST: Fast Industrial-Strength Triangulation of Polygons](http://www.cosy.sbg.ac.at/~held/projects/triang/triang.html) by Martin Held and [Triangulation by Ear Clipping](http://www.geometrictools.com/Documentation/TriangulationByEarClipping.pdf) by David Eberly - and optimized by [z-order curve](http://en.wikipedia.org/wiki/Z-order_curve) hashing.

It can handle holes, twisted polygons, degeneracies and self-intersections. While it doesn't _guarantee_ correctness of triangulation, it attempts to always produce acceptable results for practical data. In effect, it is good for *turning polygons into triangles for visualization*.

It is [faster and more correct](https://github.com/mapbox/earcut#why-another-triangulation-library) than other libraries such as libtess, poly2tri, and others.

This Zig implementation is a direct port, and should theoretically be equally correct - and possibly faster than the mapbox version.

(This repository is a separate copy of the same library in the [main Mach repository](https://github.com/hexops/mach), and is automatically kept in sync, so that anyone can use this library in their own project if they like!)

## Experimental

This is an _experimental_ Mach library, according to our [stability guarantees](https://machengine.org/next/docs/libs/):

> Experimental libraries may have their APIs change without much notice, and you may have to look at recent changes in order to update your code.

[Why this library is not declared stable yet](https://machengine.org/next/docs/libs/experimental/#earcut)

## Getting started

### Adding dependency

Create a `build.zig.zon` and add this to the `dependencies` section:
```zig
.mach_earcut = .{
    .url = "https://github.com/hexops/mach-earcut/archive/<latest commit hash>.tar.gz",
    .hash = "<get the hash by running zig build without the hash field>",
}
```

Then in your `build.zig` add:

```zig
...
pub fn build(b: *Build) void {
    ...
    exe.addModule("earcut", b.dependency("mach_earcut", .{
        .target = target,
        .optimize = optimize,
    }).module("mach-earcut"));
}
```

For usage, see `src/main.zig` `test "basic"`.

## Join the community

Join the Mach community [on Discord](https://discord.gg/XNG3NZgCqp) to discuss this project, ask questions, get help, etc.

## Issues

Issues are tracked in the [main Mach repository](https://github.com/hexops/mach/issues?q=is%3Aissue+is%3Aopen+label%3Aearcut).

## Contributing

Contributions are very welcome. Pull requests must be sent to [the main repository](https://github.com/hexops/mach/tree/main/earcut) to avoid some complex merge conflicts we'd get by accepting contributions in both repositories. Once the changes are merged there, they'll get sync'd to this repository automatically.

## Version

We currently reflect [this version](https://github.com/mapbox/earcut/tree/ae33a9fc9731c76519e66081995387e08d48eb65) of the upstream library.
