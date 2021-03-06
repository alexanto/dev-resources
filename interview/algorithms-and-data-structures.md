### Arrays 
- offer fast O(1) search if you know the index
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
   
   Output Missing numbers in integer array [1, 2, 3, 4, 6], with total number 6 is 5 Missing numbers in integer array [1, 2, 3, 4, 6, 7, 9, 8, 10], with total number 10 is 5 Missing numbers in integer array [1, 2, 3, 4, 6, 9, 8], with total number 10 is 5 7 10 Missing numbers in integer array [1, 2, 3, 4, 9, 8], with total number 10 is 5 6 7 10 Missing number in array [1, 2, 3, 5] is 4
    
  
2. **How do you find the duplicate number on a given integer array (without using Set)?**
     
     ```
     import java.util.Arrays;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    /**
     * Java program to remove duplicates from this array. You don't
     * need to physically delete duplicate elements, replacing with null, or
     * empty or default value is ok.
     *
     * @author http://javarevisited.blogspot.com
     */
    public class TechnicalInterviewTest {

        private static final Logger logger = LoggerFactory.getLogger(TechnicalInterviewTest.class);

        public static void main(String args[]) {

            int[][] test = new int[][]{
                {1, 1, 2, 2, 3, 4, 5},
                {1, 1, 1, 1, 1, 1, 1},
                {1, 2, 3, 4, 5, 6, 7},
                {1, 2, 1, 1, 1, 1, 1},};

            for (int[] input : test) {
                System.out.println("Array with Duplicates       : " + Arrays.toString(input));
                System.out.println("After removing duplicates   : " + Arrays.toString(removeDuplicates(input)));
            }
        }

        /*
         * Method to remove duplicates from array in Java, without using
         * Collection classes e.g. Set or ArrayList. Algorithm for this
         * method is simple, it first sort the array and then compare adjacent
         * objects, leaving out duplicates, which is already in the result.
         */
        public static int[] removeDuplicates(int[] numbersWithDuplicates) {

            // Sorting array to bring duplicates together      
            Arrays.sort(numbersWithDuplicates);     

            int[] result = new int[numbersWithDuplicates.length];
            int previous = numbersWithDuplicates[0];
            result[0] = previous;

            for (int i = 1; i < numbersWithDuplicates.length; i++) {
                int ch = numbersWithDuplicates[i];

                if (previous != ch) {
                    result[i] = ch;
                }
                previous = ch;
            }
            return result;

        }
    }

    Output :
    Array with Duplicates       : [1, 1, 2, 2, 3, 4, 5]
    After removing duplicates   : [1, 0, 2, 0, 3, 4, 5]
    Array with Duplicates       : [1, 1, 1, 1, 1, 1, 1]
    After removing duplicates   : [1, 0, 0, 0, 0, 0, 0]
    Array with Duplicates       : [1, 2, 3, 4, 5, 6, 7]
    After removing duplicates   : [1, 2, 3, 4, 5, 6, 7]
    Array with Duplicates       : [1, 2, 1, 1, 1, 1, 1]
    After removing duplicates   : [1, 0, 0, 0, 0, 0, 2]
    ```

3. **How do you find the largest and smallest number in an unsorted integer array?**

    ```
    import java.util.Arrays;
    /**
     * Java program to find largest and smallest number from an array in Java.
     * You cannot use any library method both from Java and third-party library.
     *
     * @author http://java67.blogspot.com
     */
    public class MaximumMinimumArrayDemo{

        public static void main(String args[]) {
            largestAndSmallest(new int[]{-20, 34, 21, -87, 92,
                                 Integer.MAX_VALUE});
            largestAndSmallest(new int[]{10, Integer.MIN_VALUE, -2});
            largestAndSmallest(new int[]{Integer.MAX_VALUE, 40,
                                 Integer.MAX_VALUE});
            largestAndSmallest(new int[]{1, -1, 0});
        }

        public static void largestAndSmallest(int[] numbers) {
            int largest = Integer.MIN_VALUE;
            int smallest = Integer.MAX_VALUE;
            for (int number : numbers) {
                if (number > largest) {
                    largest = number;
                } else if (number < smallest) {
                    smallest = number;
                }
            }

            System.out.println("Given integer array : " + Arrays.toString(numbers));
            System.out.println("Largest number in array is : " + largest);
            System.out.println("Smallest number in array is : " + smallest);
        }
    }
    Output:
    Given integer array : [-20, 34, 21, -87, 92, 2147483647]
    Largest number in array is : 2147483647
    Smallest number in array is : -87
    Given integer array : [10, -2147483648, -2]
    Largest number in array is : 10
    Smallest number in array is : -2147483648
    Given integer array : [2147483647, 40, 2147483647]
    Largest number in array is : 2147483647
    Smallest number in array is : 40
    Given integer array : [1, -1, 0]
    Largest number in array is : 1
    Smallest number in array is : -1
    ```

4. **How do you find all pairs of an integer array whose sum is equal to a given number?**

    ```
    import java.util.Arrays;
    /**
    * Java Program to find pairs on integer array whose sum is equal to k 
    * 
    * @author WINDOWS 8 
    */ 
    public class ProblemInArray{
        public static void main(String args[]) { 
            int[] numbers = { 2, 4, 3, 5, 7, 8, 9 };
            int[] numbersWithDuplicates = { 2, 4, 3, 5, 6, -2, 4, 7, 8, 9 };
            prettyPrint(numbers, 7);
            prettyPrint(numbersWithDuplicates, 7);
        }
        /**
        * Prints all pair of integer values from given array whose sum is is equal to given number. 
        * complexity of this solution is O(n^2) 
        */ 
        public static void printPairs(int[] array, int sum) {

            for (int i = 0; i < array.length; i++) {
                int first = array[i];
                    for (int j = i + 1; j < array.length; j++) {
                        int second = array[j]; 

                        if ((first + second) == sum) {
                            System.out.printf("(%d, %d) %n", first, second);
                        } 
                    } 
                } 
        } 
        /** 
        * Utility method to print input and output for better explanation. 
        */ 
        public static void prettyPrint(int[] givenArray, int givenSum){ 
            System.out.println("Given array : " + Arrays.toString(givenArray));
            System.out.println("Given sum : " + givenSum);
            System.out.println("Integer numbers, whose sum is equal to value : " + givenSum);
            printPairs(givenArray, givenSum);
        } 
    }

    Output: 
    Given sum : 7
    Integer numbers, whose sum is equal to value : 7
    (2, 5)
    (4, 3)
    Given array : [2, 4, 3, 5, 6, -2, 4, 7, 8, 9]
    Given sum : 7 
    Integer numbers, whose sum is equal to value : 7 
    (2, 5)
    (4, 3)
    (3, 4)
    (-2, 9)

    ```

    This solution is correct but it's time complexity is very hight, O(n^2), which means Interviewer will surely ask you to improve your answer and come up with solution whose complexity is either O(1), O(n) or O(nLog(n)). So let's dig deeper to improve this answer. In order to find two numbers in an array whose sum equals a given value, we probably don't need compare each number with other. What we can do here is to store all numbers in a hashtable and just check if it contains second value in a pair. For example, if given sum is 4 and one number in pair is 3, then other must be 1 or -7. Do you remember the first question we asked, if array only contains positive numbers then we don't need to check for negative values in Map. How is this solution better than previous one? It would require less comparisons. Only N to iterate through array and insert values in a Set because add() and contains() both O(1) operation in hash table. So total complexity of solution would be O(N). Here is a Java program which find the pair of values in the array whose sum is equal to k using Hashtable or Set. In this program we have also written a utility method to generate random numbers in a given range in Java. You can use this method for testing with random inputs. By the way, random numbers are only good for demonstration, don't use them in your unit test. One more good thing you can learn from printPairsUsingSet() method is pre validation, checking if inputs are valid to proceed further.

    ```
    import java.util.Arrays;
    import java.util.HashSet;
    import java.util.Set;

    /** 
    * Java Program to find two elements in an array that sum to k. 
    * 
    * @author WINDOWS 8 
    */ 
    public class ArraySumUsingSet {

        public static void main(String args[]) {
            prettyPrint(getRandomArray(9), 11);
            prettyPrint(getRandomArray(10), 12);
        } 

        /** 
        * Given an array of integers finds two elements in the array whose sum is equal to n. 
        * @param numbers 
        * @param n 
        */ 
        public static void printPairsUsingSet(int[] numbers, int n){
            if(numbers.length < 2){
                return;
            } 
            Set set = new HashSet(numbers.length);

            for(int value : numbers){
                int target = n - value;

                // if target number is not in set then add 
                if(!set.contains(target)){
                    set.add(value);
                }else { 
                    System.out.printf("(%d, %d) %n", value, target);
                } 
            } 
        } 

        /* 
        * Utility method to find two elements in an array that sum to k. 
        */ 
        public static void prettyPrint(int[] random, int k){
            System.out.println("Random Integer array : " + Arrays.toString(random)); 
            System.out.println("Sum : " + k); 
            System.out.println("pair of numbers from an array whose sum equals " + k);
            printPairsUsingSet(random, k);
        } 
        /** 
        * Utility method to return random array of Integers in a range of 0 to 15 
        */ 
        public static int[] getRandomArray(int length){
            int[] randoms = new int[length];
            for(int i=0; i<length; i++){
                randoms[i] = (int) (Math.random()*15); 
            }
                return randoms;
        } 
    } 

    Output: 
    Random Integer array : [0, 14, 0, 4, 7, 8, 3, 5, 7] 
    Sum : 11 
    pair of numbers from an array whose sum equals 11 
    (7, 4) 
    (3, 8) 
    (7, 4) 
    Random Integer array : [10, 9, 5, 9, 0, 10, 2, 10, 1, 9] 
    Sum : 12 
    pair of numbers from an array whose sum equals 12 
    (2, 10)

    ```


5. **How do you find duplicate numbers in an array if it contains multiple duplicates?**

    ```
    import java.io.IOException; import java.util.ArrayList; import java.util.HashMap; import java.util.HashSet; import java.util.LinkedHashMap; import java.util.List; import java.util.Map; import java.util.Map.Entry; import java.util.Set; /** * Java Program to find first duplicate, non-repeated character in a String. * It demonstrate three simple example to do this programming problem. * * @author Javarevisited */ public class Programming { /* * Using LinkedHashMap to find first non repeated character of String * Algorithm : * Step 1: get character array and loop through it to build a * hash table with char and their count. * Step 2: loop through LinkedHashMap to find an entry with * value 1, that's your first non-repeated character, * as LinkedHashMap maintains insertion order. */ public static char getFirstNonRepeatedChar(String str) { Map<Character,Integer> counts = new LinkedHashMap<>(str.length()); for (char c : str.toCharArray()) { counts.put(c, counts.containsKey(c) ? counts.get(c) + 1 : 1); } for (Entry<Character,Integer> entry : counts.entrySet()) { if (entry.getValue() == 1) { return entry.getKey(); } } throw new RuntimeException("didn't find any non repeated Character"); } /* * Finds first non repeated character in a String in just one pass. * It uses two storage to cut down one iteration, standard space vs time * trade-off.Since we store repeated and non-repeated character separately, * at the end of iteration, first element from List is our first non * repeated character from String. */ public static char firstNonRepeatingChar(String word) { Set<Character> repeating = new HashSet<>(); List<Character> nonRepeating = new ArrayList<>(); for (int i = 0; i < word.length(); i++) { char letter = word.charAt(i); if (repeating.contains(letter)) { continue; } if (nonRepeating.contains(letter)) { nonRepeating.remove((Character) letter); repeating.add(letter); } else { nonRepeating.add(letter); } } return nonRepeating.get(0); } /* * Using HashMap to find first non-repeated character from String in Java. * Algorithm : * Step 1 : Scan String and store count of each character in HashMap * Step 2 : traverse String and get count for each character from Map. * Since we are going through String from first to last character, * when count for any character is 1, we break, it's the first * non repeated character. Here order is achieved by going * through String again. */ public static char firstNonRepeatedCharacter(String word) { HashMap<Character,Integer> scoreboard = new HashMap<>(); // build table [char -> count] for (int i = 0; i < word.length(); i++) { char c = word.charAt(i); if (scoreboard.containsKey(c)) { scoreboard.put(c, scoreboard.get(c) + 1); } else { scoreboard.put(c, 1); } } // since HashMap doesn't maintain order, going through string again for (int i = 0; i < word.length(); i++) { char c = word.charAt(i); if (scoreboard.get(c) == 1) { return c; } } throw new RuntimeException("Undefined behaviour"); } }

    ```

6. **How are duplicates removed from a given array in Java?**

    ```
    import java.util.Arrays;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    /**
     * Java program to remove duplicates from this array. You don't
     * need to physically delete duplicate elements, replacing with null, or
     * empty or default value is ok.
     *
     * @author http://javarevisited.blogspot.com
     */
    public class TechnicalInterviewTest {

        private static final Logger logger = LoggerFactory.getLogger(TechnicalInterviewTest.class);

        public static void main(String args[]) {

            int[][] test = new int[][]{
                {1, 1, 2, 2, 3, 4, 5},
                {1, 1, 1, 1, 1, 1, 1},
                {1, 2, 3, 4, 5, 6, 7},
                {1, 2, 1, 1, 1, 1, 1},};

            for (int[] input : test) {
                System.out.println("Array with Duplicates       : " + Arrays.toString(input));
                System.out.println("After removing duplicates   : " + Arrays.toString(removeDuplicates(input)));
            }
        }

        /*
         * Method to remove duplicates from array in Java, without using
         * Collection classes e.g. Set or ArrayList. Algorithm for this
         * method is simple, it first sort the array and then compare adjacent
         * objects, leaving out duplicates, which is already in the result.
         */
        public static int[] removeDuplicates(int[] numbersWithDuplicates) {

            // Sorting array to bring duplicates together      
            Arrays.sort(numbersWithDuplicates);     

            int[] result = new int[numbersWithDuplicates.length];
            int previous = numbersWithDuplicates[0];
            result[0] = previous;

            for (int i = 1; i < numbersWithDuplicates.length; i++) {
                int ch = numbersWithDuplicates[i];

                if (previous != ch) {
                    result[i] = ch;
                }
                previous = ch;
            }
            return result;

        }
    }

    Output :
    Array with Duplicates       : [1, 1, 2, 2, 3, 4, 5]
    After removing duplicates   : [1, 0, 2, 0, 3, 4, 5]
    Array with Duplicates       : [1, 1, 1, 1, 1, 1, 1]
    After removing duplicates   : [1, 0, 0, 0, 0, 0, 0]
    Array with Duplicates       : [1, 2, 3, 4, 5, 6, 7]
    After removing duplicates   : [1, 2, 3, 4, 5, 6, 7]
    Array with Duplicates       : [1, 2, 1, 1, 1, 1, 1]
    After removing duplicates   : [1, 0, 0, 0, 0, 0, 2]

    ```

7. **How is an integer array sorted in place using the quicksort algorithm?**

    ```
    import java.util.Arrays; /** * Test class to sort array of integers using Quicksort algorithm in Java. * @author Javin Paul */ public class QuickSortDemo{ public static void main(String args[]) { // unsorted integer array int[] unsorted = {6, 5, 3, 1, 8, 7, 2, 4}; System.out.println("Unsorted array :" + Arrays.toString(unsorted)); QuickSort algorithm = new QuickSort(); // sorting integer array using quicksort algorithm algorithm.sort(unsorted); // printing sorted array System.out.println("Sorted array :" + Arrays.toString(unsorted)); } } /** * Java Program sort numbers using QuickSort Algorithm. QuickSort is a divide * and conquer algorithm, which divides the original list, sort it and then * merge it to create sorted output. * * @author Javin Paul */ class QuickSort { private int input[]; private int length; public void sort(int[] numbers) { if (numbers == null || numbers.length == 0) { return; } this.input = numbers; length = numbers.length; quickSort(0, length - 1); } /* * This method implements in-place quicksort algorithm recursively. */ private void quickSort(int low, int high) { int i = low; int j = high; // pivot is middle index int pivot = input[low + (high - low) / 2]; // Divide into two arrays while (i <= j) { /** * As shown in above image, In each iteration, we will identify a * number from left side which is greater then the pivot value, and * a number from right side which is less then the pivot value. Once * search is complete, we can swap both numbers. */ while (input[i] < pivot) { i++; } while (input[j] > pivot) { j--; } if (i <= j) { swap(i, j); // move index to next position on both sides i++; j--; } } // calls quickSort() method recursively if (low < j) { quickSort(low, j); } if (i < high) { quickSort(i, high); } } private void swap(int i, int j) { int temp = input[i]; input[i] = input[j]; input[j] = temp; } } Output : Unsorted array :[6, 5, 3, 1, 8, 7, 2, 4] Sorted array :[1, 2, 3, 4, 5, 6, 7, 8]

    ```

    Import points about Quicksort algorithm
    Now we know how quick sort works and how to implement quicksort in Java, its time to revise some of the important points about this popular sorting algorithm.

    1) QuickSort is a divide and conquer algorithm. Large list is divided into two and sorted separately (conquered), sorted list is merge later.

    2) On "in-place" implementation of quick sort, list is sorted using same array, no additional array is required. Numbers are re-arranged pivot, also known as partitioning.

    3) Partitioning happen around pivot, which is usually middle element of array.

    4) Average case time complexity of Quicksort is O(n log n) and worst case time complexity is O(n ^2), which makes it one of the fasted sorting algorithm. Interesting thing is it's worst case performance is equal to Bubble Sort :)

    5) Quicksort can be implemented with an in-place partitioning algorithm, so the entire sort can be done with only O(log n) additional space used by the stack during the recursion.

    6) Quicksort is also a good example of algorithm which makes best use of CPU caches, because of it's divide and conquer nature.

    7) In Java, Arrays.sort() method uses quick sort algorithm to sort array of primitives. It's different than our algorithm, and uses two pivots. Good thing is that it perform much better than most of the quicksort algorithm available on internet for different data sets, where traditional quick sort perform poorly. One more reason, not to reinvent the wheel but to use the library method, when it comes to write production code.


8. **How do you remove duplicates from an array in place?**

    ```
    import java.util.Arrays;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    /**
     * Java program to remove duplicates from this array. You don't
     * need to physically delete duplicate elements, replacing with null, or
     * empty or default value is ok.
     *
     * @author http://javarevisited.blogspot.com
     */
    public class TechnicalInterviewTest {

        private static final Logger logger = LoggerFactory.getLogger(TechnicalInterviewTest.class);

        public static void main(String args[]) {

            int[][] test = new int[][]{
                {1, 1, 2, 2, 3, 4, 5},
                {1, 1, 1, 1, 1, 1, 1},
                {1, 2, 3, 4, 5, 6, 7},
                {1, 2, 1, 1, 1, 1, 1},};

            for (int[] input : test) {
                System.out.println("Array with Duplicates       : " + Arrays.toString(input));
                System.out.println("After removing duplicates   : " + Arrays.toString(removeDuplicates(input)));
            }
        }

        /*
         * Method to remove duplicates from array in Java, without using
         * Collection classes e.g. Set or ArrayList. Algorithm for this
         * method is simple, it first sort the array and then compare adjacent
         * objects, leaving out duplicates, which is already in the result.
         */
        public static int[] removeDuplicates(int[] numbersWithDuplicates) {

            // Sorting array to bring duplicates together      
            Arrays.sort(numbersWithDuplicates);     

            int[] result = new int[numbersWithDuplicates.length];
            int previous = numbersWithDuplicates[0];
            result[0] = previous;

            for (int i = 1; i < numbersWithDuplicates.length; i++) {
                int ch = numbersWithDuplicates[i];

                if (previous != ch) {
                    result[i] = ch;
                }
                previous = ch;
            }
            return result;

        }
    }

    Output :
    Array with Duplicates       : [1, 1, 2, 2, 3, 4, 5]
    After removing duplicates   : [1, 0, 2, 0, 3, 4, 5]
    Array with Duplicates       : [1, 1, 1, 1, 1, 1, 1]
    After removing duplicates   : [1, 0, 0, 0, 0, 0, 0]
    Array with Duplicates       : [1, 2, 3, 4, 5, 6, 7]
    After removing duplicates   : [1, 2, 3, 4, 5, 6, 7]
    Array with Duplicates       : [1, 2, 1, 1, 1, 1, 1]
    After removing duplicates   : [1, 0, 0, 0, 0, 0, 2]

    ```

9. **How do you reverse an array in place in Java?**

    ```
    import java.util.Arrays; import org.apache.commons.lang.ArrayUtils; /** * * Java program to reverse array using Apache commons ArrayUtils class. * In this example we will reverse both int array and String array to * show how to reverse both primitive and object array in Java. * * @author */ public class ReverseArrayExample { public static void main(String args[]) { int[] iArray = new int[] {101,102,103,104,105}; String[] sArray = new String[] {"one", "two", "three", "four", "five"}; // reverse int array using Apache commons ArrayUtils.reverse() method System.out.println("Original int array : " + Arrays.toString(iArray)); ArrayUtils.reverse(iArray); System.out.println("reversed int array : " + Arrays.toString(iArray)); // reverse String array using ArrayUtis class System.out.println("Original String array : " + Arrays.toString(sArray)); ArrayUtils.reverse(sArray); System.out.println("reversed String array in Java : " + Arrays.toString(sArray)); } } Output: Original int array : [101, 102, 103, 104, 105] reversed int array : [105, 104, 103, 102, 101] Original String array : [one, two, three, four, five] reversed String array in Java : [five, four, three, two, one]
    ```

10. **How are duplicates removed from an array without using any library?**

    ```
    import java.util.Arrays;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    /**
     * Java program to remove duplicates from this array. You don't
     * need to physically delete duplicate elements, replacing with null, or
     * empty or default value is ok.
     *
     * @author http://javarevisited.blogspot.com
     */
    public class TechnicalInterviewTest {

        private static final Logger logger = LoggerFactory.getLogger(TechnicalInterviewTest.class);

        public static void main(String args[]) {

            int[][] test = new int[][]{
                {1, 1, 2, 2, 3, 4, 5},
                {1, 1, 1, 1, 1, 1, 1},
                {1, 2, 3, 4, 5, 6, 7},
                {1, 2, 1, 1, 1, 1, 1},};

            for (int[] input : test) {
                System.out.println("Array with Duplicates       : " + Arrays.toString(input));
                System.out.println("After removing duplicates   : " + Arrays.toString(removeDuplicates(input)));
            }
        }

        /*
         * Method to remove duplicates from array in Java, without using
         * Collection classes e.g. Set or ArrayList. Algorithm for this
         * method is simple, it first sort the array and then compare adjacent
         * objects, leaving out duplicates, which is already in the result.
         */
        public static int[] removeDuplicates(int[] numbersWithDuplicates) {

            // Sorting array to bring duplicates together      
            Arrays.sort(numbersWithDuplicates);     

            int[] result = new int[numbersWithDuplicates.length];
            int previous = numbersWithDuplicates[0];
            result[0] = previous;

            for (int i = 1; i < numbersWithDuplicates.length; i++) {
                int ch = numbersWithDuplicates[i];

                if (previous != ch) {
                    result[i] = ch;
                }
                previous = ch;
            }
            return result;

        }
    }

    Output :
    Array with Duplicates       : [1, 1, 2, 2, 3, 4, 5]
    After removing duplicates   : [1, 0, 2, 0, 3, 4, 5]
    Array with Duplicates       : [1, 1, 1, 1, 1, 1, 1]
    After removing duplicates   : [1, 0, 0, 0, 0, 0, 0]
    Array with Duplicates       : [1, 2, 3, 4, 5, 6, 7]
    After removing duplicates   : [1, 2, 3, 4, 5, 6, 7]
    Array with Duplicates       : [1, 2, 1, 1, 1, 1, 1]
    After removing duplicates   : [1, 0, 0, 0, 0, 0, 2]
    ```
    
