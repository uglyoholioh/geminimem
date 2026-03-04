# NUSMods Style Timetable Refactor

The timetable rendering logic has been rewritten to perfectly match the NUSMods experience.

## Changes Made
- **Responsive "Zero-Scroll" Layout**: The `table` in the horizontal view drops its minimum width limits and now uses `table-fixed w-full`. This ensures the table will **always** shrink or stretch to fit 100% of the screen width exactly, without throwing horizontal scrollbars.
- **Dimensional Event Spanning**: Event blocks are now rendered with `absolute` positioning instead of block `relative` stacking.
- **Accurate Event Ratios**: 
  - An event's physical rendering `width` automatically calculates based on its true `spanHrs`. A 2-hour class will now precisely span across two 1-hour table columns.
  - Events offset their `left` accurately via `offsetHrs`. A class starting at 10:30 will accurately render exactly halfway across the 10:00 table cell.
- **Conflict Staggering**: Concurrent classes now elegantly overlap with a slight `4px` cascade stagger (rather than blowing up the height of the row or obscuring each other entirely).
- **Vertical View Parity**: The same coordinate math (`top`, `height`, `left` staggering) was seamlessly ported to the vertical view (Days on X, Time on Y) to ensure visual consistency regardless of which orientation is toggled.

## Validation
- The frontend will auto-refresh. The timetable should now compactly squeeze to 100% viewport width and correctly visualize durations.
- No scrollbar will appear under the horizontal view.
