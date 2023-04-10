>https://www.youtube.com/watch?v=tD5NrevFtbU

**Traditionally**
1. Polymorphism is good - if/switch is bad
2. No internals - things should be abstracted and call say a polymorphic function in an object. Never look into object to see what's inside the object.
3. Functions should be small.
4. Functions should do one thing.
5. D.R.Y.

In general if you take all of these things - they are fairly specific about how code should be written to be "clean". 

**But how does it perform?**

Example using traditional example of a Base Shape Class:
In the loop to calculate area - we don't know what shape we will be getting - so we have to use a "Shape" pointer.

**Two tests:**
- Cold entry - no cache - like L3 - first time running timing tests.
- Cached entry - possibly L2 - second time running.

Clean version took around ~35 cycles.

**Let's violate the rules and see if we get a performance gain:**
1. Just use switch statements

Benefits:
1. Do not need to pass a Shape pointer to function - can simply pass the array of Shapes.
2. Compiler can see exactly what it is doing because it doesn't have to wonder about what kind of types might be derived from a base class.

Immediate improvement of around 1.5x. ~24 cycles. Almost equivalent of going from a modern iPhone to an old iPhone.

Architechrually with a switch statement you can see all the conditions up front - whereas with polymorphic implementation - they would be scattered between files for each Shape type.

**Let's violate the rules and see if we get a performance gain:**
1. Use a lookup table instead of a switch statement.

Immediate improvement of around 10x. ~3.5 cycles. Around 12 years of hardware evolution itself. Massive gain.

**Let's violate the rules again with another operation added in for more complexity to see if we get a performance gain:**

This time we will add another part of the computation that will take each endpoints (Triangle 3, Square 4) - and make them weighted - adding to the calculation.

**Outcome:**
- Clean code way performs even worse. Considering now the new functionality calls for a new individual virtual function.
- ~15x speed enhancement using tables - also code is ironically "cleaner".

This is without optimizations such as SSE or AVX. With those you can get up to ~25x enhancement.

**Conclusion**
1. ~~Polymorphism is good - if/switch is bad~~
2. ~~No internals - things should be abstracted and call say a polymorphic function in an object. Never look into object to see what's inside the object.~~
3. ~~Functions should be small.~~
4. ~~Functions should do one thing.~~
5. D.R.Y. - should think about - but isn't ALWAYS the best. Sometimes repeating is worth it for performance based on the needs.
