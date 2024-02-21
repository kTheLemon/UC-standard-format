# UC
The UC specification.

# Structure
UC is structured similarly to other formats; starting with `UC;` to mark the code as a UC code. The structure is the following: `UC;<grid_data>;<bg_cell_dictionary><bg_cells>;<normal_cell_dictionary><normal_cells>;<fg_cell_dictionary><fg_cells>;<ffg_cell_dictionary><ffg_cells>...`

## Grid Data
Grid data contains all the grid's data:
```json
{
  "width": 100,
  "height": 100,
  "title": "Title",
  "desc": "Description",
  "remake": "The Remake Machine Cell"
}
```

## Cell Dictionaries
Cell dictionaries are used to compactify the level code. They allow the cell lists to compactify the ID's as much as possible. The ID's will be turned into ASCII characters (using a function equivalent to the lua `string.char`), so you'll need to seperate them by bytes. (1 byte goes up to 255). These new ID's must start at 1 to avoid null bytes. It is recommended to make lower numbers more common cells to optimise the level code, although this is not required.
Here's an example:
```json
{
  "my_epic_cell": 1,
  "ultra_mega_puller": 2
}
```

## Storing Cells
Cells are stored like this:
```
<rot>:<id>:<vars?>,<rot>:<id>:<vars?>,<rot>:<id>:<vars?>,...
```
Cell variables are optional but are there if needed. However, they take up a lot of space so using them sparingly is recommended. Here's an example of a right-facing mover (assuming its id in the dictionary is 65, the ASCII value for `A`):
```
0:A,
```

# Compressing
Everything after the `UC;` header must be compressed through zlib then Base64-encoded. If this step isn't taken then the header must be modified to be `-UC;` instead.

# Universal Cell ID's
Some cells have universal ID's. Those should be stored as raw text, while cells with non-universal ID's should have a `+` at the start.
The list of universal ID's is:
- `empty` means an empty cell (AKA nothing),
- `mover` means the standard cell machine mover,
- `gen` means the standard cell machine generator,
- `rotcw` means the standard clockwise cell machine rotator,
- `rotccw` means the standard counterclockwise cell machine rotator,
- `rot180` means the standard 180-degree cell machine rotator,
- `push` means the standard cell machine push cell,
- `slide` means the standard cell machine 2-sided push cell with parallel pushable sides,
- `wall` means the standard cell machine unpushable cell,
- `trash` means the standard cell machine trash cell,
- `enemy` means the standard cell machine enemy,

## Backgrounds

- `place` means a placeable spot.
- `void` means a background that looks like the outside of the grid.

## Extension
- `ghost` means an ungeneratable wall,
- `freeze` means a cell that stops all orthogonally adjacent cells from updating,
- `shield` means a cell that prevents all orthogonally adjacent destroyer cells (cells that can be destroyed from being moved into) from being destroyed and destroying,
- `push1` means a one-directional cell that in its default rotation can only be pushed to the right,
- `push2` means a two-directional cell that in its default rotation can be pushed to the right or upwards,
- `push3` means a three-directional cell that in its default rotation can be pushed to the right, upwards or downwards,
- `rep` means a pushable cell that pushed a copy of the cell in front of it to its front (kinda like a front-to-front generator),
- `enemy:n` means an enemy with n amount of health (can destroy n cells before dying) (weak enemy is `enemy:0`, normal is `enemy:1`, strong is `enemy:2`, etc...),
- `bomb:n` means a bomb with a radius of n (`bomb:0` is an enemy, `bomb:1` is a bomb with a 3x3 effect area, `bomb:2` is a bomb with a 5x5 effect area, etc...),
- `weakwall` means a wall that can only be destroyed by using bombs.
