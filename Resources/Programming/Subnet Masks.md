>[ip - How do you calculate the prefix, network, subnet, and host numbers? - Network Engineering Stack Exchange](https://networkengineering.stackexchange.com/questions/7106/how-do-you-calculate-the-prefix-network-subnet-and-host-numbers/7117#7117)

## Calculating the Netmask Length (also called a prefix):

Convert the dotted-decimal representation of the netmask to binary. Then, count the number of contiguous 1 bits, starting at the most significant bit in the first octet (i.e. the left-hand-side of the binary number).

```
255.255.248.0   in binary: 11111111 11111111 11111000 00000000
                           -----------------------------------
                           I counted twenty-one 1s             -------> /21
```

The prefix of 128.42.5.4 with a 255.255.248.0 netmask is /21.

## Calculating the Network Address:

The network address is the logical AND of the respective bits in the binary representation of the IP address and network mask. Align the bits in both addresses, and perform a logical AND on each pair of the respective bits. Then convert the individual octets of the result back to decimal.

Logical AND truth table:

![Logical AND](https://i.stack.imgur.com/e5tIQ.png)

```
128.42.5.4      in binary: 10000000 00101010 00000101 00000100
255.255.248.0   in binary: 11111111 11111111 11111000 00000000
                           ----------------------------------- [Logical AND]
                           10000000 00101010 00000000 00000000 ------> 128.42.0.0
```

As you can see, the network address of 128.42.5.4/21 is 128.42.0.0

## Calculating the Broadcast Address:

The broadcast address converts all host bits to 1s...

Remember that our IP address in decimal is:

```
128.42.5.4      in binary: 10000000 00101010 00000101 00000100
```

The network mask is:

```
255.255.248.0   in binary: 11111111 11111111 11111000 00000000
```

This means our host bits are the last 11 bits of the IP address, because we find the host mask by inverting the network mask:

```
Host bit mask            : 00000000 00000000 00000hhh hhhhhhhh
```

To calculate the broadcast address, we force all host bits to be 1s:

```
128.42.5.4      in binary: 10000000 00101010 00000101 00000100
Host bit mask            : 00000000 00000000 00000hhh hhhhhhhh
                           ----------------------------------- [Force host bits]
                           10000000 00101010 00000111 11111111 ----> 128.42.7.255
```

## Calculating subnets:

You haven't given enough information to calculate subnets for this network; as a general rule you build subnets by reallocating some of the host bits as network bits for each subnet. Many times there isn't one right way to subnet a block... depending on your constraints, there could be several valid ways to subnet a block of addresses.

Let's assume we will break 128.42.0.0/21 into 4 subnets that must hold at least 100 hosts each...

![subnetting](https://i.stack.imgur.com/wHlF8.png)

In this example, we know that you need at least a /25 prefix to contain 100 hosts; I chose a /24 because it falls on an octet boundary. Notice that the network address for each subnet borrows host bits from the parent network block.

### Finding the required subnet masklength or netmask:

How did I know that I need at least a /25 masklength for 100 hosts? Calculate the prefix by backing into the number of host bits required to contain 100 hosts. One needs 7 host bits to contain 100 hosts. Officially this is calculated with:

_Host bits_ = Log2(Number-of-hosts) = Log2(100) = 6.643

Since IPv4 addresses are 32 bits wide, and we are using the host bits (i.e. least significant bits), simply subtract 7 from 32 to calculate the minimum subnet prefix for each subnet... 32 - 7 = 25.

### The lazy way to break 128.42.0.0/21 into four equal subnets:

Since we only want four subnets from the whole 128.42.0.0/21 block, we could use /23 subnets. I chose /23 because we need 4 subnets... i.e. an extra two bits added to the netmask.

This is an equally-valid answer to the constraint, using /23 subnets of 128.42.0.0/21...

![subnetting, 2nd option](https://i.stack.imgur.com/nKRVg.png)

## Calculating the host number:

This is what we've already done above... just reuse the host mask from the work we did when we calculated the broadcast address of 128.42.5.4/21... This time I'll use 1s instead of `h`, because we need to perform a logical AND on the network address again.

```
128.42.5.4      in binary: 10000000 00101010 00000101 00000100
Host bit mask            : 00000000 00000000 00000111 11111111
                           ----------------------------------- [Logical AND]
                           00000000 00000000 00000101 00000100 -----> 0.0.5.4
```

## Calculating the maximum possible number of hosts in a subnet:

To find the maximum number of hosts, look at the number of binary bits in the host number above. The easiest way to do this is to subtract the netmask length from 32 (number of bits in an IPv4 address). This gives you the number of host bits in the address. At that point...

_Maximum Number of hosts_ = 2**(32 - netmask_length) - 2

The reason we subtract 2 above is because the all-ones and all-zeros host numbers are reserved. The all-zeros host number is the network number; the all-ones host number is the broadcast address.

Using the example subnet of 128.42.0.0/21 above, the number of hosts is...

_Maximum Number of hosts_ = 2**(32 - 21) - 2 = 2048 - 2 = 2046

## Finding the maximum netmask (minimum hostmask) which contains two IP addresses:

Suppose someone gives us two IP addresses and expects us to find the longest netmask which contains both of them; for example, what if we had:

-   128.42.5.17
-   128.42.5.67

The easiest thing to do is to convert both to binary and look for the longest string of network-bits from the left-hand side of the address.

```
128.42.5.17     in binary: 10000000 00101010 00000101 00010001
128.42.5.67     in binary: 10000000 00101010 00000101 01000011
                           ^                           ^     ^
                           |                           |     |
                           +--------- Network ---------+Host-+
                             (All bits are the same)    Bits
```

In this case the maximum netmask (minimum hostmask) would be /25

NOTE: If you try starting from the right-hand side, don't get tricked just because you find one matching column of bits; there could be unmatched bits beyond those matching bits. Honestly, the safest thing to do is to start from the left-hand side.