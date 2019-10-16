### Arrays 
- offer dasf O(1) search if you know the index
- adding and removing is slow because you cannot change the size of the array once it is created (must create new array and copy all the elements)

1. **How do you find the missing number in a given integer array of 1 to 100?**
    
    One of the most frequently asked question on programming interviews is, write a program to find the missing number in an array in Java, C# or any other language; depending upon which language you choose. This kind of coding interview questions are not only asked in small start-ups but also on some of the biggest technical companies like Google, Amazon, Facebook, Microsoft, mostly when they visit the campus of reputed universities to hire graduates. Simplest version of this question is to find missing elements in an area of 100 integers, which contains numbers between 1 and 100. This can easily be solved by calculating the sum of the series using n(n+1)/2, and this is also one of the quickest and efficient ways, but it cannot be used if the array contains more than one missing numbers or if the array contains duplicates.
    This gives interviewer some nice follow-up questions to check whether a candidate can apply his knowledge of the slightly different condition or not. So if you get through this, they will ask you to find the missing number in an array of duplicates. This might be tricky but you will soon find out that another way to find missing and duplicate number in the array is to sort it.
    In a sorted array, you can compare whether a number is equal to expected next number or not. Alternatively, you can also use BitSet in Java to solve this problem.
    Read more: https://javarevisited.blogspot.com/2014/11/how-to-find-missing-number-on-integer-array-java.html#ixzz62VJYtyDR
    
    Let's understand the problem statement, we have numbers from 1 to 100 that are put into an integer array, what's the best way to find out which number is missing? If Interviewer especially mentions 1 to 100 then you can apply the above trick about the sum of the series as shown below as well. If it has more than one missing element that you can use BitSet class, of course only if your interviewer allows it.

    1) Sum of the series: Formula: n (n+1)/2( but only work for one missing number)
    2) Use BitSet, if an array has more than one missing elements.

    I have provided a BitSet solution with another purpose, to introduce with this nice utility class. In many interviews, I have asked about this class to Java developers, but many of them not even aware of this. I think this problem is a nice way to learn how to use BitSet in Java as well.

    By the way, if you are going for interview, then apart from this question, its also good to know how to find duplicate number in array and program to find second highest number in an integer array. More often than not, those are asked as follow-up question after this.

    Read more: https://javarevisited.blogspot.com/2014/11/how-to-find-missing-number-on-integer-array-java.html#ixzz62VJiOujR
    
    ```
    import java.util.Arrays;
    import java.util.BitSet;

    /**
     * Java program to find missing elements in a Integer array containing 
     * numbers from 1 to 100.
     *
     * @author Javin Paul
     */
    public class MissingNumberInArray {

        public static void main(String args[]) {

            // one missing number
            printMissingNumber(new int[]{1, 2, 3, 4, 6}, 6);

            // two missing number
            printMissingNumber(new int[]{1, 2, 3, 4, 6, 7, 9, 8, 10}, 10);

            // three missing number
            printMissingNumber(new int[]{1, 2, 3, 4, 6, 9, 8}, 10);

            // four missing number
            printMissingNumber(new int[]{1, 2, 3, 4, 9, 8}, 10);

            // Only one missing number in array
            int[] iArray = new int[]{1, 2, 3, 5};
            int missing = getMissingNumber(iArray, 5);
            System.out.printf("Missing number in array %s is %d %n", 
                               Arrays.toString(iArray), missing);
        }
       /**
        * A general method to find missing values from an integer array in Java.
        * This method will work even if array has more than one missing element.
        */
        private static void printMissingNumber(int[] numbers, int count) {
            int missingCount = count - numbers.length;
            BitSet bitSet = new BitSet(count);

            for (int number : numbers) {
                bitSet.set(number - 1);
            }

            System.out.printf("Missing numbers in integer array %s, with total number %d is %n",
            Arrays.toString(numbers), count);
            int lastMissingIndex = 0;

            for (int i = 0; i < missingCount; i++) {
                lastMissingIndex = bitSet.nextClearBit(lastMissingIndex);
                System.out.println(++lastMissingIndex);
            }

        }
       /**
        * Java method to find missing number in array of size n containing
        * numbers from 1 to n only.
        * can be used to find missing elements on integer array of 
        * numbers from 1 to 100 or 1 - 1000
        */
        private static int getMissingNumber(int[] numbers, int totalCount) {
            int expectedSum = totalCount * ((totalCount + 1) / 2);
            int actualSum = 0;
            for (int i : numbers) {
                actualSum += i;
            }

            return expectedSum - actualSum;
        }

    }
   ```
    
  
2. **How do you find the duplicate number on a given integer array?**

3. **How do you find the largest and smallest number in an unsorted integer array?**

4. **How do you find all pairs of an integer array whose sum is equal to a given number?**

5. **How do you find duplicate numbers in an array if it contains multiple duplicates?**

6. **How are duplicates removed from a given array in Java?**

7. **How is an integer array sorted in place using the quicksort algorithm?**

8. **How do you remove duplicates from an array in place?**

9. **How do you reverse an array in place in Java?**

10. **How are duplicates removed from an array without using any library?**
