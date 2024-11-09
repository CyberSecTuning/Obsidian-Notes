[[Assembly Cheat Sheet]]
[[Registers]]
## Address Endianness

An address endianness is the order of its bytes in which they are stored or retrieved from memory. There are two types of endianness: `Little-Endian` and `Big-Endian`. With Little-Endian processors, the little-end byte of the address is filled/retrieved first `right-to-left`, while with Big-Endian processors, the big-end byte is filled/retrieved first `left-to-right`.

For example, if we have the address `0x0011223344556677` to be stored in memory, little-endian processors would store the `0x00` byte on the right-most bytes, and then the `0x11` byte would be filled after it, so it becomes `0x1100`, and then the `0x22` byte, so it becomes `0x221100`, and so on. Once all bytes are in place, they would look like `0x7766554433221100`, which is the reverse of the original value. Of course, when retrieving the value back, the processor will also use little-endian retrieval, so the value retrieved would be the same as the original value.

Another example that shows how this can affect the stored values is binary. For example, if we had the 2-byte integer `426`, its binary representation is `00000001 10101010`. The order in which these two bytes are stored would change its value. For example, if we stored it in reverse as `10101010 00000001`, its value becomes `43521`.

The big-endian processors would store these bytes as `00000001 10101010` `left-to-right`, while little-endian processors store them as `10101010 00000001` `right-to-left`. When retrieving the value, the processor has to use the same endianness used when storing them, or it will get the wrong value. This indicates that the order in which the bytes are stored/retrieved makes a big difference.

The following table demonstrates how endianness works:

|**Address**|**0**|**1**|**2**|**3**|**4**|**5**|**6**|**7**|**Address Value**|
|---|---|---|---|---|---|---|---|---|---|
|**Little Endian**|77|66|55|44|33|22|11|00|0x7766554433221100|
|**Big Endian**|00|11|22|33|44|55|66|77|0x0011223344556677|