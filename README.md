# NTS Trap Designer

A self-contained web application for visualizing trapezoid nesting on rectangular steel plates.

## Features

- **Single & Double Nesting Modes**: Place one or two trapezoids on a plate
- **Real-time Validation**: Input validation with inline error messages (max 4 decimal places)
- **Smart Nesting**: Automatic head-to-foot arrangement for optimal space utilization
- **Visual Feedback**: Clear indication when shapes don't fit (red dashed outlines)
- **Dimension Display**: Real-time plate dimensions, trapezoid areas, and utilization percentage
- **SVG Export**: Download your designs as SVG files
- **Responsive Design**: Clean, accessible interface with keyboard support
- **Preset Examples**: Three built-in test cases to get started quickly

## Usage

### Quick Start

1. Open `index.html` in any modern web browser
2. Enter plate dimensions (length, width, thickness) in inches
3. Enter trapezoid dimensions (top width, bottom width, height)
4. Click **Draw** to visualize the nesting arrangement
5. Optionally enable **Double Nest Mode** to add a second trapezoid
6. Click **Download SVG** to save your design

### Input Fields

**Plate Dimensions:**
- Length, Width, Thickness (all in inches)

**Trapezoid Dimensions:**
- Top Width: Width of the narrow end
- Bottom Width: Width of the wide end
- Height: Distance between parallel edges
- Orientation: Position of narrow end (left/right)

**Options:**
- Margin: Clearance from plate edges (default 0.125")
- Gap: Clearance between shapes in double mode (default 0.125")

### Preset Examples

Click any preset button to auto-fill the form:

1. **Example 1**: Single trapezoid that fits (96×48 plate, 6/18/36 trap)
2. **Example 2**: Double nest that fits (96×48 plate, two trapezoids)
3. **Example 3**: Single trapezoid that doesn't fit (24×12 plate, oversized trap)

### Validation Rules

- All dimensions must be positive numbers
- Maximum 4 decimal places allowed
- Shapes that exceed plate bounds are shown with red dashed outlines
- Real-world dimensions are preserved (no auto-scaling to force fit)

## Technical Details

- **Single HTML file**: No build tools or external dependencies required
- **Vanilla JavaScript**: No frameworks, just clean ES6+ code
- **SVG rendering**: Scalable vector graphics with uniform scaling
- **Responsive layout**: Works on desktop and tablet screens
- **Accessible**: Keyboard navigation, ARIA labels, focus management

## Nesting Logic

**Single Mode:**
- Centers trapezoid on plate with specified margin
- Validates fit within plate bounds

**Double Mode:**
- Head-to-foot arrangement (narrow end meets wide end)
- Attempts lengthwise placement first
- Falls back to widthwise placement if needed
- Shows "Does not fit" warning if neither works
- Maintains specified gap between shapes

## Material Utilization

The app calculates and displays:
- Individual trapezoid areas
- Total plate area
- Utilization percentage (trapped area / plate area × 100)

## License

See LICENSE file for details.
