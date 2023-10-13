File: readme.txt
Author: Miles Zoltak
----------------------

1) Exploring the real world
   My pink CASIO fx-300ES PLUS
      This is a pretty basic calculator, not used for graphing or anything, but a little better
      than a TI-30x IIS, but not by much.  Really just your run-of-the-mill pre-calc calculator.
      That being said, it doesn't seem to suffer from some floating point issues that the computer
      I am using to do this assignment falls victim to.  No matter the order in which I calculate
      sequence 6 from problem 2, I get the right answer.  Where it says 2^24 + 1 (16777217) should
      not be representable by floating point, this calculator has no problem with the calculation.
      The calculator starts having trouble when we have greater than 12 significant digits it seems.
      See the below equations demonstrated by the calculator:
          1e10 + 0.001 - 1e10 = 0
          1e11 + 0.01 - 1e11 = 0
          1e12 + 0.1 - 1e12 = 0
          1e13 + 1 - 1e13 = 0
      However this is where things get weird...
          1e10 + 0.001 - 1e10 = 0
          but......
          1e10 + 0.011 - 1e10 = 0.011
          ...I would have at least expected to get 0.010 but that really threw me for a loop.
     I know that when I try to execute an expression whose answer is outside of the representable
     range it just spits out "MATH ERROR", so that's how it deals with that.  The same error is
     thrown for inputs such as sqrt(-1) and 0/0, so those are the exceptional values it seems.  It
     doesn't have a way of representing infinity, or at least it doesn't output something that tells
     you it is trying to do so.
     We dicussed above (indented portion) the computer's choosiness about precision in decimals, but
     those were large numbers.  When I do very small numbers I can represent 1e-99 at the smallest.
     However, if I try to add a whole number to that I am limited to only 10 significant digits.
     For instance:
         1e-10 + 1 = 1
         1e-9 + 1 = 1.000000001 (there are 8 zeros)
     As for 32-bit or 64-bit structures, I haven't been able to discern that yet.  I tried to take
     the back off to see if I could get an idea from looking at the circuitry, specifically if there
     was anything revealing explicitly written on the chip.  I wouldn't have actually been able to
     discern anything solely from the chip.  Unfortunately, I don't really feel comfortable using
     what I've observed to conclude what system is being used but it doesn't seem to be the same as
     the system we've been discussing in class.  I would say at least in part the simplicity of the
     display is to blame.  It's only 15 1's across and not multiline.  I think, however, that this
     is also a way of handling some of the inaccuracy due to computing constraints.  In the past I
     just figured the display was too small so it had to switch to scientific notation, in some
     cases at the expense of seeing significant digits, but now I see that the issue is that the
     calculator can't actually store that precision.  It has been very eye-opening but I still don't
     know exactly what is going on.

   Microsoft Excel
      OK SOMETHING REALLY COOL IS HAPPENING!
      In fiddling around with Excel at first I have noticed behavior also exhibited by my little
      pink calculator!  If I set the format for the cells I'm working with as "General" then the
      following behavior can be observed:
          Take the expressioin 1 + 1e-n where n is in the range [1, inf]
          For n = [0, 9] the expression is computed as excpected BUT...
          For n > 9 we just get 1!!!
      This is just like good ol' pinky (as demonstrated in the previous section)!  The main
      difference is that if I change the cell format to "Number" there is a lot more precision
      that can be seen, but only up to n = 14 which means there are 15 significant digits.  This
      tracks perfectly with the information on Microsoft's website which says that number precision
      is limited to 15 digits!  I couldn't help but peeking and it also shows a lot of other
      representation limitations, such as the maximum and minimum numbers which are equivalent to
      that of double, not float.  Therefore, we can assume that Excel is using a 64-bit
      representation.  I do wonder what would happen if we were using it on a 32-bit architecture,
      I would imagine that it ~should~ stay the same, but I'm not sure how that would work.  Maybe
      I'm mistaken, but I thought my old laptop before this (circa 2013) was 32-bits, Excel worked
      just fine but I know I never pushed it thaaaaat hard.
      If I try to calculate numbers outside of the range a variety of things could happen, depending
      on how hard I am pushing the boundaries of the system:
         If I add 1 to the maximum number, it simply doesn't register the 1 and rounds back down to
         the max.  However if I do something really crazy like mulitply that number by 2, it gives
         the #NUM! error.  This error also appears in cases where I try to compute an exceptional
         number like sqrt(-1).  0/0 gets its very own error message (#DIV/0!), which is probably
         especially relevant because of the likelihood a mistyped formula could come upon this
         specific error a good deal more often than sqrt(-1) or 9.99999999999999e308 * 2, so it
         deserves its own personal error message.
      But yeah I see that it has some properties similar to my calculator, but definitely more
      powerful beacuse it has the power of a 64-bit float behind it!

      One issue that still remains unresolved for me is why 11 digits seems to be the issue for my
      calculator as well as numbers under the "General" format in Excel.  This doesn't seem to be
      indicative of a plain 32-bit float because that should be limited to only 7 digits.
     
2) Floating average
   Average A
           The alternative method "averageA" uses to compute the average is to do the division
           for each individual element in the array and then sum them up as you go.  This method
           of computing the average works well in sequence 1 where none of the numbers are
           particularly exceptional, but all of the methods do so that's not very interesting.
           The strengths of this method are demonstrated when extremely large numbers are present
           in the sequence.  Whereas all of the other methods add all numbers and then divide,
           this method divides along the way to keep the sum from reaching inf when the average
           really isn't inf.  Unfortunately, for more mundane numbers this method can present
           problems with rounding errors.  For instance if you have 10 numbers and any one of them
           is not divisible by 5, you're going to have rounding error because floats can't represent
           tenths.
   Average B
           In "averageB" we use qsort to first sort the sequence in ascending order, summing them
           up, and then dividing by the number of elements.  This is, in some cases, functionally
           the same as the naive version but the order in which elements are summed can make a real
           difference with floats.  The strength of this method is demonstrated when there are very
           large numbers along with smaller more, more mundane numbers in the sequence.  Take
           sequence 6, where there is one large number and along with it 0, -1, and 1.  The order
           here matters because when we add small numbers to large numbers we are likely to run
           into rounding errors.  When we keep the small numbers in order with the larger numbers
           (eg: -1, 0, 1, 2^24) by the time we add 2^24 to the sum, we are only adding it to 0 and
           so we don't have to risk rounding error which would certainly happened if we tried to
           add or subtract 1 from such a large number.  Still, this won't work as well if there are
           very large positive AND negative numbers along with the smaller ones because the small
           numbers break up the large ones because the large are different signs.
   Average C
           In "averageC" we see a somewhat similar approach, except instead of being sorted in
           ascending order, the comparison function passed into qsort returns copy[] in descending
           order of ABSOLUTE VALUE.  The choice to use absolute value addresses exactly the weakness
           we see in "averageB".  This is demonstrated in sequence 7, where only "averageC" computes
           the correct value because the two very large numbers are summed first and they add up to
           zero (because they only differ in sign) and then the rest of the numbers are smaller and
           do not cause rounding errors.  The choice to use reverse order of absolute value is done
           for this exact reason.  Reverse order addresses the very large numbers first, and when
           these numbers differ in sign things work out quite nicely because their sum will be
           reasonably small (especially in the controlled environment of our test sequences).  The
           flipside of this method is that when the large values don't cancel each other out we then
           might go ahead and start summing large values with small ones which can cause rounding
           errors.  Again, this just demonstrates how the order of addition can make a difference
           depending on the size and sign of the values when we are working with floats.  See below
           for an example.
           {-1, 0, 1, 1 << 24} --> averageB --> CORRECT
           {-1, 0, 1, 1 << 24} --> averageC --> INCORRECT
                averageB works well because the summing in ascending order means the first 3 numbers
                will cancel out to 0 and then all that's left to be added is 1 << 24.
                On the other hand, averageC doesn't work because it adds 1 to 1 << 24 which results
                in a rounding error.  Even though we then add -1 right after that, the damage is done
                and we're working with corrupted values.
           {1.5e8, -1.5e8, 100.5, 99.5} --> averageB --> INCORRECT
           {1.5e8, -1.5e8, 100.5, 99.5} --> averageC --> CORRECT
                averageB doesn't work here because 99.5 gets added to -1.5e8 and we have a rounding
                error, corrupting the subsequent calculations.
                averageC on the other hand handles this very well because  1.5e8 and -1.5e8 cancel
                out and the rest of the summing can be done easily because 99.5 and 100.5 are small
                enough to not have rounding errors.
         
3) bbsort
3a)  The comparison function is called <compare_keys>
     There are 44 assembly instructions in <compare_keys>
3b)
    p *(char **)$rdi
    p *(char **)$rsi
3c)
    The global variable is named option_mask32 and it lives at 0x603180
    For the each flag option_mask32 has the following value:
        -n = 1
        -u = 8
        -r = 256
    When combining flags, option_mask32's value is just the sum of the values of each flag, thus
    -nru = 265 because 1 + 8 + 256 = 265.
3d)
    (-r) After passing the array through qsort, the array is sorted in reverse order.  I'm not
         really sure what needs explaining here, but one thing to note was that it wasn't just
         reversing the order of the file but the sort order as compared to no flags or the -n flag.
    (-u) The duplicates were not removed after sorting.  There were still several reds that followed
         the first (even though they were all at the very end).  I suppose this would indicate that
         there is some logic within bbsort that is doing the uniqeness selection then instead of
         qsort.
    (-n) The lines are repeatedly converted to a number on compare.  I can tell because when I do
         "run -n colors" in gdb breaking on qsort and printing shows that the values are indeed
         char *'s and not ints.  That is, when I use the p command I have to do *((char **)b)
         instead of *(int *)b or something like that.

