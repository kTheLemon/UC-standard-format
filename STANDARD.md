# UC
The UC specification.

# Structure
UC is structured similarly to other formats; starting with `UC;` to mark the code as a UC code. The structure is the following: `UC;<grid_data>;<normal_cell_dictionary><normal_cells>;<bg_cell_dictionary><bg_cells>;<fg_cell_dictionary><fg_cells>;`

## Grid Data
Grid data contains all the grid's data:
```json
{
  "width": 100,
  "height": 100,
  "title": "Title",
  "desc": "Description"
}
```

## Cell Dictionaries
Cell dictionaries are used to compactify the level code. They allow the cell lists to compactify the ID's as much as possible. The ID's will be turned into ASCII characters, so you'll need to seperate them by bytes. (1 byte goes up to 255). These new ID's must start at 1 to avoid null bytes.
Here's an example:
```json
{
  "my_epic_cell": 1,
  "ultra_mega_puller": 2
}
```
