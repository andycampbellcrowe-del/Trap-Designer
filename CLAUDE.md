# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

NTS Trap Designer is a single-file web application for visualizing trapezoid nesting on rectangular steel plates. The entire application is contained in `index.html` with no external dependencies, build tools, or frameworks.

## Running the Application

Simply open `index.html` in any modern web browser. For development:

```bash
python3 -m http.server 8080
```

Then navigate to `http://localhost:8080` in your browser.

## Architecture

The application is structured as a single HTML file with three embedded sections:

### 1. CSS (lines 28-367)
- Responsive two-column layout (left panel: form inputs, right panel: SVG canvas)
- Mobile breakpoint at 1024px collapses to vertical stack
- Light theme with purple gradient header (#667eea to #764ba2)
- Trapezoid colors: blue (A: rgba(59, 130, 246, 0.6)), orange (B: rgba(249, 115, 22, 0.6))

### 2. HTML (lines 369-579)
- Left panel: Form with plate dimensions, trapezoid dimensions, options, and preset buttons
- Right panel: SVG canvas (800x600 viewBox) and info footer with statistics
- Double nest mode conditionally shows/hides Trapezoid B inputs

### 3. JavaScript (lines 581-1322)
Organized into five functional sections:

**Utility Functions (lines 586-652)**
- `parseInch(value)`: Validates positive numbers with ≤4 decimal places
- `formatInch(value, dp)`: Formats numbers for display
- `computeScale(plateW, plateL, svgW, svgH, paddingPct)`: Calculates uniform scale factor (5% padding)
- `exportSVG(svgEl, filename)`: Downloads SVG file

**Geometry Functions (lines 658-717)**
- `calculateTrapezoidPoints(topW, bottomW, height, orientation)`: Generates 4-point isosceles trapezoid polygon
  - Centers narrower edge over wider edge
  - Orientation ('left'/'right') determines narrow end position
  - Returns `{points, width, height}` where width is the max of top/bottom widths
- `calculateTrapezoidArea(topW, bottomW, height)`: Standard trapezoid area formula
- `checkFit(trap, plateL, plateW, margin)`: Boolean fit check accounting for margin

**Nesting Logic (lines 723-814)**
- `nestSingle(trap, plateL, plateW, margin)`: Centers single trapezoid on plate
- `nestDouble(trapA, trapB, plateL, plateW, margin, gap)`: Head-to-foot arrangement
  - Priority 1: Lengthwise placement (stacked along plate length)
  - Priority 2: Widthwise placement (side-by-side along plate width) — **not fully implemented**
  - Returns `{fits: boolean, trapezoids: Array}` with calculated positions
  - Note: Widthwise placement (lines 786-799) is a stub; currently only lengthwise works

**Drawing Functions (lines 820-990)**
- SVG constants: `SVG_WIDTH=800`, `SVG_HEIGHT=600`, `PADDING_PCT=0.05`
- `draw(data)`: Main drawing orchestrator
- `drawGrid()`: Renders 1" grid lines (heavier every 5"), labeled every 5"
- `drawPlate()`: Rounded rectangle outline
- `drawTrapezoid()`: Polygon with drop shadow filter (defined in SVG defs)
  - Error state: lighter fill, red dashed stroke, no shadow
  - Includes SVG `<title>` element for hover tooltips

**Form Handling (lines 996-1321)**
- Real-time validation on blur events
- `validateInput()`: Individual field validation with inline error messages
- `processAndDraw()`: Main workflow — validate → calculate geometry → nest → compute scale → draw → update info
- Three preset examples populate form with test cases

## Key Constraints

1. **Input Validation**: All dimensions must be positive numbers with max 4 decimal places
2. **Uniform Scaling**: X and Y axes use the same scale factor (no distortion)
3. **No Auto-fit**: Shapes are never resized to force fit; "Does not fit" warning shown instead
4. **Isosceles Trapezoids**: Two non-parallel sides are always equal length
5. **Coordinate System**: Trapezoid local coordinates have Y-axis pointing up, origin at base

## Dimensions and Units

- All input/output in inches (with up to 4 decimal precision)
- Default margin: 0.125"
- Default gap: 0.125"
- Area calculated in square inches
- Utilization: (total trapezoid area / plate area) × 100%

## Common Modifications

### Adding New Nesting Strategies
Modify `nestDouble()` function (lines 759-814). Current implementation only supports lengthwise stacking. Widthwise placement stub exists but requires rotation logic.

### Changing Colors
Update color values:
- Trapezoid A: line 862 (`rgba(59, 130, 246, 0.6)`)
- Trapezoid B: line 862 (`rgba(249, 115, 22, 0.6)`)
- Legend: lines 568, 572 (HTML)

### Modifying Grid
Adjust `drawGrid()` function (lines 873-928):
- Line spacing: loops increment by 1 (inch)
- Label spacing: `if (i % 5 === 0)` controls 5" intervals
- Opacity: line 875 (`group.setAttribute('opacity', '0.2')`)

### Adding New Shapes
1. Create geometry function similar to `calculateTrapezoidPoints()`
2. Update `processAndDraw()` to handle new shape type
3. Add form inputs with validation
4. Modify drawing logic to support new polygon types

## Testing

Three preset examples available in UI (lines 1278-1321):
1. **Preset 1**: 96×48×0.375 plate, 6/18/36 trap (fits)
2. **Preset 2**: 96×48×0.375 plate, double nest 6/18/36 + 10/14/36 (fits)
3. **Preset 3**: 24×12×0.25 plate, 14/20/18 trap (doesn't fit)

## Known Limitations

- Widthwise placement in double nest mode is not fully implemented
- Only supports up to 2 trapezoids (not extensible to N shapes)
- No rotation support (shapes are always horizontal)
- No optimization algorithm (uses fixed head-to-foot strategy)
