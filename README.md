# bigfish
largest fish in OoT

More details: https://www.youtube.com/watch?v=Hgo4vQSgtyY

OoT decomp: https://github.com/zeldaret/oot

Thanks to the decomp team!!

Lines that were modified from [z_fishing.c](https://github.com/zeldaret/oot/blob/3d61fb85efa13203db2e4888c9e892833a164277/src/overlays/actors/ovl_Fishing/z_fishing.c): 832 to 837, 1042, 1045 and 1046

# results
1. Adult Link regular fish max size: 25 pounds (1/1600 probability)

2. Child Link regular fish max size: 13 pounds (1/158 probability)

3. Adult Link Loach max size: 35 pounds (1/5 probability)

4. Child Link Loach max size: 19 pounds (1/7 probability)

# probabilities explanation
Every line of code in the following refers to [z_fishing.c](https://github.com/zeldaret/oot/blob/3d61fb85efa13203db2e4888c9e892833a164277/src/overlays/actors/ovl_Fishing/z_fishing.c) from the decomp's github linked above. We assume uniform distributions for the random floats, but a more accurate way would be to go through all 2^32 possible RNG seeds.


**1. Adult Link regular fish**

Base weight is 71 ([line 836](https://github.com/zeldaret/oot/blob/3d61fb85efa13203db2e4888c9e892833a164277/src/overlays/actors/ovl_Fishing/z_fishing.c#L836)). A random float from 0 to 5 gets added to this weight ([line 1050](https://github.com/zeldaret/oot/blob/3d61fb85efa13203db2e4888c9e892833a164277/src/overlays/actors/ovl_Fishing/z_fishing.c#L1050)). Let's denote this float by x. With 1/20 probability ([line 1053](https://github.com/zeldaret/oot/blob/3d61fb85efa13203db2e4888c9e892833a164277/src/overlays/actors/ovl_Fishing/z_fishing.c#L1053)) a random float from 0 to 8 gets added to the weight as well ([line 1054](https://github.com/zeldaret/oot/blob/3d61fb85efa13203db2e4888c9e892833a164277/src/overlays/actors/ovl_Fishing/z_fishing.c#L1054)). Let's denote this second float by y.

The formula to convert this weight into pounds is given by floor(floor(71 + x + y)^2 * 0.0036 + 0.5) ([line 5757](https://github.com/zeldaret/oot/blob/3d61fb85efa13203db2e4888c9e892833a164277/src/overlays/actors/ovl_Fishing/z_fishing.c#L5757)). The floor functions come from the conversions from floats to integers. We compute the maximum weight in pounds using x + y = 13, which gives 25. Then we want to compute the minimal value of x + y such that the weight in pounds is 25. This gives a triangular region in the 5×8 rectangle of possibilities for x and y. This triangular region is where the weight of the fish in pounds is 25, any other values of x and y will give 24 or less. Compute the areas and divide to get the probability. Don't forget to multiply by 1/20 as well to get the final probability of 1/1600.


**2. Child Link regular fish**

The total weight of a fish is multiplied by 0.73 since Link is a child and fish are smaller ([line 1065](https://github.com/zeldaret/oot/blob/3d61fb85efa13203db2e4888c9e892833a164277/src/overlays/actors/ovl_Fishing/z_fishing.c#L1065)). The pounds formula is then floor(floor((71 + x + y) * 0.73)^2 * 0.0036 + 0.5). Repeat the logic described above.


**3. Adult Link Loach**

Base weight is 45 ([line 837](https://github.com/zeldaret/oot/blob/3d61fb85efa13203db2e4888c9e892833a164277/src/overlays/actors/ovl_Fishing/z_fishing.c#L837)). A random float from 0 to 5 gets added to this weight ([line 1050](https://github.com/zeldaret/oot/blob/3d61fb85efa13203db2e4888c9e892833a164277/src/overlays/actors/ovl_Fishing/z_fishing.c#L1050)). The second float doesn't happen for the Loach ([line 1053](https://github.com/zeldaret/oot/blob/3d61fb85efa13203db2e4888c9e892833a164277/src/overlays/actors/ovl_Fishing/z_fishing.c#L1053)), so no 1/20 extra factor in the probability. Its weight is multiplied by 2 ([line 3952](https://github.com/zeldaret/oot/blob/3d61fb85efa13203db2e4888c9e892833a164277/src/overlays/actors/ovl_Fishing/z_fishing.c#L3952)). The pound formula is then floor(floor((45 + x) * 2)^2 * 0.0036 + 0.5). Repeat the logic above, it's simpler cause you only have a single variable. You might think 36 pounds is the maximum, but the maximum x can go is actually 4.99999, not 5 ([line 1050](https://github.com/zeldaret/oot/blob/3d61fb85efa13203db2e4888c9e892833a164277/src/overlays/actors/ovl_Fishing/z_fishing.c#L1050)).

**4. Child Link Loach**

The pound formula is floor(floor((45 + x) * 2 * 0.73)^2 * 0.0036 + 0.5).
