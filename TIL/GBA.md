# Learn About GBA Basic And Some Low Level Things
Today(2026-05-30) I Learn How GBA Rendering A Tile Graphics,Gba Have 8x8 Tile Size Or 8 Row 8 Column,if we assign helper for mapping to a tile data it would be look like this

---
    map_to_tile_data[1][i] = 0x04040404;
---

We use 2d Arrays For It,the 1st array is our Index of tile,its like an Id so i we can access it from memory.The second array is our array indices,i assign the **typedef** in Zig like this,

---
    pub const Tile = [16]u32;
---

Before that i taking notes how the Tile is worked in 8 Bit Per Pixel Mode or 8bpp.

1 pixel = 1 byte/8bits

GBA Have 32 Bits Instruction CPU(ARM7TDMI) that mean the **word** size is 32 Bits or 4 Bytes.

VRAM Need **unsigned 32bits Integer** or **u32** that mean it equals to **4 Bytes** or **1 Word**

Because the size of Tile in GBA is 8x8 pixel that mean 8 bytes x 8 bytes equals to **64 Bytes**.
      
So to fill each block(1 word) in VRAM we need substract the tile data size with our word that means.

---
    64 Bytes(1 Tile) / 4 Bytes(1 Word) = 16 Blocks/Words
---
     
In My Code Using While Loop

---
    while(i < 16):(i+=1){
        map_to_tile_data[1][i] = 0x04030404;
    }
---

We loop 16 Times because we need to fill our 16 blocks with a tile that contain the pallete color.

This The Layout Of The Tile Inside VRAM

**Tile Layout**

---
    tile[0]   tile[1]   -> row 0
    tile[2]   tile[3]   -> row 1
    tile[4]   tile[5]   -> row 2
    tile[6]   tile[7]   -> row 3
    tile[8]   tile[9]   -> row 4
    tile[10]  tile[11]  -> row 5
    tile[12]  tile[13]  -> row 6
    tile[14]  tile[15]  -> row 7
---

More Ergonomics Its Visualized Like This
            
**Tile Layout**

---
    Left Half (even index)     Right Half (odd index)
    Row 0:    [ 4 ][ 4 ][ 4 ][ 4 ]    |    [ 4 ][ 4 ][ 4 ][ 4 ]   <- Solid Top Cap
    Row 1:    [ 4 ][ 4 ][ 0 ][ 0 ]    |    [ 0 ][ 0 ][ 4 ][ 4 ]   <- Hollow Walls
    Row 2:    [ 4 ][ 4 ][ 0 ][ 0 ]    |    [ 0 ][ 0 ][ 4 ][ 4 ]
    Row 3:    [ 4 ][ 4 ][ 0 ][ 0 ]    |    [ 0 ][ 0 ][ 4 ][ 4 ]
    Row 4:    [ 4 ][ 4 ][ 0 ][ 0 ]    |    [ 0 ][ 0 ][ 4 ][ 4 ]
    Row 5:    [ 4 ][ 4 ][ 0 ][ 0 ]    |    [ 0 ][ 0 ][ 4 ][ 4 ]
    Row 6:    [ 4 ][ 4 ][ 0 ][ 0 ]    |    [ 0 ][ 0 ][ 4 ][ 4 ]
    Row 7:    [ 4 ][ 4 ][ 4 ][ 4 ]    |    [ 4 ][ 4 ][ 4 ][ 4 ]   <- Solid Bottom Cap
---

Total Row = 8

Total Column = 8

Total Pixel Data = 64

**This Is Beatiful Of Knowledge**
