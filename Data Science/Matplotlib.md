1. allow for interactive, cross-platform control of figures and plots
2. make it easy to produce static raster or vector graphics files without the need for any GUIs.

## Anatomy of a "Plot"

- The `Figure` is the top-level container in this hierarchy. It is the overall window/page that everything is drawn on. You can have multiple independent figures and `Figure`s can contain multiple `Axes`.
- Most plotting ocurs on an `Axes`. The axes is effectively the area that we plot data on and any ticks/labels/etc associated with it. Usually we'll set up an Axes with a call to `subplot` (which places Axes on a regular grid), so in most cases, `Axes` and `Subplot` are synonymous.
- Each `Axes` has an `XAxis` and a `YAxis`. These contain the ticks, tick locations, labels, etc. In this tutorial, we'll mostly control ticks, tick labels, and data limits through other mechanisms, so we won't touch the individual `Axis` part of things all that much. However, it is worth mentioning here to explain where the term `Axes` comes from.

